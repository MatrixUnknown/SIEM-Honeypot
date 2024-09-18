| extend username = extract(@"username:([^,]+)", 1, RawData),
         timestamp = extract(@"timestamp:([^,]+)", 1, RawData),
         latitude = extract(@"latitude:([^,]+)", 1, RawData),
         longitude = extract(@"longitude:([^,]+)", 1, RawData),
         sourcehost = extract(@"sourcehost:([^,]+)", 1, RawData),
         state = extract(@"state:([^,]+)", 1, RawData),
         label = extract(@"label:([^,]+)", 1, RawData),
         destination = extract(@"destinationhost:([^,]+)", 1, RawData),
         country = extract(@"country:([^,]+)", 1, RawData)
| where destination != "samplehost"
| where sourcehost != ""
| summarize event_count=count() by latitude, longitude, sourcehost, label, destination, country






| parse RawData with * "latitude:" Latitude ",longitude:" Longitude ",destinationhost:" DestinationHost ",username:" Username ",sourcehost:" SourceHost ",state:" State ", country:" Country ",label:" Label ",timestamp:" Timestamp
| project Latitude, Longitude, DestinationHost, Username, SourceHost, State, Country, Label, Timestamp
| summarize event_count=count() by SourceHost, Latitude, Longitude, Country, Label, DestinationHost 
| where DestinationHost != "samplehost"
| where SourceHost != ""
