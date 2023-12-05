## User:
```JSON
{
	"_id": objectID,
	"email": string,
	"password": string,
	"type": string,
	"name": string,
	"surname": string,
	"img": binary (optional),
	"review": {
		"avarage": number,
		"total": number
	}(optional),
	"availability": [{
		"latitude": number,
		"longitude": number,
		"starDate": date,
		"endDate": date
	}] (optional),
}
```

## Trip:
```JSON
{
	"_id": ObjectID,
	"driverId": ObjectID,
	"userId": ObjectID,
	"amount": number,
	"name": string,
	"surname": string,
	"img": binary,
	"review": {
		"avarage": number,
		"total": number
	},
	"pickUpTime": date,
	"arrivalTime": date,
	"distance": number
}
```
## Ban
```JSON
{
	"_id": ObjectID,
	"email": string,
	"motivation": string (optional)
}
```
## Review
```JSON
{
	"_id": ObjectID,
	"taxiId": ObjectID,
	"date": date,
	"rating": number,
	"comment": string
}
```