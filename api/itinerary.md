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
| Name      | Type                                                |
|-----------| ----------------------------------------------------|
| name      | String (this can be any arbitrary string - optional)|
| documents | Array of Strings                                    |

## Stops - Available settings
| Name    | Type                                |
|---------|-------------------------------------|
| country | String (ISO 3166-1 alpha-2)         |
| reason  | String (tourist, business, student) |

## API Response

The API will respond with a very long message, encoded as json, with an array that contains the following information each passenger:

* `name` - Name of the passenger
* `documents` - The documents they have (passports or Visa) - this basically parrots the information you provided in your request.
  * `name` of the document
  * `type` whether the document is a passport or a visa
  * `id` the ID you sent
* `stops` a list of all the stops on the itinerary (every passenger has a full itinerary)
  * `country` contains the information of the country to be visited
  * `granted` contains a boolean flag whether the user can access the country without additional visa
  * `candidates` contains a list of visa and passport combinations the customer could use to access the country.
    * `documents`: every candidate contains a list of documents that make it viable to satisfy the condition of entering the country.
      * `identifier`  is a unique ID for this document
      * `name` contains a friendly name for the product (this is localized)
      * `recommended` allows highlighting commonly requested Visa
      * `product` contains (for products that can be acquired) a set of basic information on the product.
  * `grants` if granted is true, this contains the set of documents the customer will need to access the country.


## Example of an itinerary as a request:

This is an example request that the API would receive from the client consuming the API. Your application needs to send the request with the HTTP header `Content-type: application/json` for the system to properly process the information.

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


The response you will receive from the server looks like this:

```json
{
  "payload": [
    {
      "name": "Test",
      "documents": [ //A list of the resolved documents in your request
        {
          "name": "Pass (ANDORRA)",
          "type": "passport",
          "id": "PAD"
        },
        {...}
      ],
      "stops": [ //The result of our lookup
        {
          "country": { //The country checked
            "ISO": "AU",
            "name": "AUSTRALIEN"
          },
          "reason": "business",
          "granted": false,
          "candidates": [ //A list of possible combinations of documents to enter the country
            {
              "documents": [
                {
                  "identifier": "VAUBB90",
                  "name": "Australien Temporary Work (Short Stay Specialist) Visum (bis 90 Tage)",
                  "recommended": false,
                  "product": {
                    "id": 293,
                    "url": "https:\/\/www.visa-gate.com\/node\/293",
                    "price": {
                      "amt": "29552",
                      "natural": "295.52",
                      "currency": "EUR"
                    }
                  }
                },
                {
                  "identifier": "PAD",
                  "name": "Pass (ANDORRA)",
                  "recommended": false,
                  "product": null
                }
              ]
            }
          ],
          "grants": null,
          "_rid": "511",
          "evaluated": 219
        },
        {
          "country": {
            "ISO": "DE",
            "name": "DEUTSCHLAND"
          },
          "reason": "tourist",
          "granted": true,
          "candidates": [],
          "grants": [
            [
              [
                "PAD",
                "Pass (ANDORRA)"
              ]
            ]
          ],
          "_rid": "75",
          "evaluated": 95
        },
        {
          "country": {
            "ISO": "US",
            "name": "VEREINIGTE STAATEN VON AMERIKA"
          },
          "reason": "tourist",
          "granted": false,
          "candidates": [
            {
              "documents": [
                {
                  "identifier": "VUSTO90",
                  "name": "USA ESTA Visum (max. 90 Tage) (bis 90 Tage)",
                  "recommended": false,
                  "product": {
                    "id": 56,
                    "url": "https:\/\/www.visa-gate.com\/node\/56",
                    "price": {
                      "amt": "3900",
                      "natural": "39.00",
                      "currency": "EUR"
                    }
                  }
                },
                {
                  "identifier": "PAD",
                  "name": "Pass (ANDORRA)",
                  "recommended": false,
                  "product": null
                }
              ]
            },
            {
              "documents": [
                {
                  "identifier": "VUSTO6M",
                  "name": "USA B1 \/ B2 Visum (bis 6 Monate)",
                  "recommended": false,
                  "product": {
                    "id": 554,
                    "url": "https:\/\/www.visa-gate.com\/node\/554",
                    "price": {
                      "amt": "26900",
                      "natural": "269.00",
                      "currency": "EUR"
                    }
                  }
                }
              ]
            }
          ],
          "grants": null,
          "_rid": "555",
          "evaluated": 45
        }
      ]
    },
    {
      //This contains the data for the second passenger
    }
  ],
  "request": "..."
}
```