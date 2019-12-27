# Visa-Gate API Server - Itinerary
For us an itinerary is a set of people and a set of destinations to travel to.
Based on those information the API can generate a response with the information if a visa is needed for this person and destination or not.

## Table of Contents
- [Visa-Gate API Server - Itinerary](#visa-gate-api-server---itinerary)
	- [Table of Contents](#table-of-contents)
	- [People - Available settings](#people---available-settings)
	- [Stops - Available settings](#stops---available-settings)
	- [Example of an itinerary as a request:](#example-of-an-itinerary-as-a-request)

## People - Available settings
| Name      | Type             |
|-----------|------------------|
| name      | String           |
| documents | Array of Strings |

## Stops - Available settings
| Name    | Type                                |
|---------|-------------------------------------|
| country | String (ISO 3166-1 alpha-2)         |
| reason  | String (tourist, business, student) |

## Example of an itinerary as a request:
```json
{
	"people": [
		{
			"name" : "Passenger 1",
			"documents" : ["PAD","VAFBB30"]
		}
		{
			"name" : "Passenger 2",
			"documents" : ["PAF"]
		}
	],
	"stops": [
		{"country" : "AU", "reason" : "business"},
		{"country" : "AF", "reason" : "business"},
		{"country" : "AD", "reason" : "business"},
		{"country" : "DE", "reason" : "tourist"},
		{"country" : "US", "reason" : "tourist"}
	]
}
```
