# Visa-Gate API Server - PDF generation
This endpoint generates a PDF-file based on the information given in the JSON-request.

## Table of Contents
- [Visa-Gate API Server - PDF generation](#visa-gate-api-server---pdf-generation)
	- [Table of Contents](#table-of-contents)
	- [Example of a JSON-request](#example-of-a-json-request)
	- [Request:](#request)

## Example of a JSON-request
| Type           | Value                                         |
|----------------|-----------------------------------------------|
| Endpoint       | `https://pdf.stage.visa-gate.com/genrate/pdf` |
| Request Method | POST                                          |
| Request Type   | JSON                                          |

## Request:
```json
{
	"people": [
		{
			"name" : "Test-User",
			"documents" : ["PAD","VAFBB30"]
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
