# Exploring JSON with jq: Learning by Doing

Dataset taken from: https://www.kaggle.com/datasets/yelp-dataset/yelp-dataset?resource=download&select=yelp_academic_dataset_tip.json

```
% TMPJSON='{"a": 5, "b": 6, "c": {"x": "string1", "y": "string2", "z": [2,4,6,8,10]}}'
% echo $TMPJSON
{"a": 5, "b": 6, "c": {"x": "string1", "y": "string2", "z": [2,4,6,8,10]}}
```

```
% echo $TMPJSON | jq '.'
{
  "a": 5,
  "b": 6,
  "c": {
    "x": "string1",
    "y": "string2",
    "z": [
      2,
      4,
      6,
      8,
      10
    ]
  }
}
```

```
% echo $TMPJSON | jq '. | keys'
[
  "a",
  "b",
  "c"
]
```

```
% echo $TMPJSON | jq '. | keys | length'
3
```

```
% echo $TMPJSON | jq '. | keys | .[:2]'
[
  "a",
  "b"
]
```

```
mv yelp_academic_dataset_business.json data.json
```

```
% head -3 data.json
{"business_id":"Pns2l4eNsfO8kk83dixA6A","name":"Abby Rappoport, LAC, CMQ","address":"1616 Chapala St, Ste 2","city":"Santa Barbara","state":"CA","postal_code":"93101","latitude":34.4266787,"longitude":-119.7111968,"stars":5.0,"review_count":7,"is_open":0,"attributes":{"ByAppointmentOnly":"True"},"categories":"Doctors, Traditional Chinese Medicine, Naturopathic\/Holistic, Acupuncture, Health & Medical, Nutritionists","hours":null}
{"business_id":"mpf3x-BjTdTEA3yCZrAYPw","name":"The UPS Store","address":"87 Grasso Plaza Shopping Center","city":"Affton","state":"MO","postal_code":"63123","latitude":38.551126,"longitude":-90.335695,"stars":3.0,"review_count":15,"is_open":1,"attributes":{"BusinessAcceptsCreditCards":"True"},"categories":"Shipping Centers, Local Services, Notaries, Mailbox Centers, Printing Services","hours":{"Monday":"0:0-0:0","Tuesday":"8:0-18:30","Wednesday":"8:0-18:30","Thursday":"8:0-18:30","Friday":"8:0-18:30","Saturday":"8:0-14:0"}}
{"business_id":"tUFrWirKiKi_TAnsVWINQQ","name":"Target","address":"5255 E Broadway Blvd","city":"Tucson","state":"AZ","postal_code":"85711","latitude":32.223236,"longitude":-110.880452,"stars":3.5,"review_count":22,"is_open":0,"attributes":{"BikeParking":"True","BusinessAcceptsCreditCards":"True","RestaurantsPriceRange2":"2","CoatCheck":"False","RestaurantsTakeOut":"False","RestaurantsDelivery":"False","Caters":"False","WiFi":"u'no'","BusinessParking":"{'garage': False, 'street': False, 'validated': False, 'lot': True, 'valet': False}","WheelchairAccessible":"True","HappyHour":"False","OutdoorSeating":"False","HasTV":"False","RestaurantsReservations":"False","DogsAllowed":"False","ByAppointmentOnly":"False"},"categories":"Department Stores, Shopping, Fashion, Home & Garden, Electronics, Furniture Stores","hours":{"Monday":"8:0-22:0","Tuesday":"8:0-22:0","Wednesday":"8:0-22:0","Thursday":"8:0-22:0","Friday":"8:0-23:0","Saturday":"8:0-23:0","Sunday":"8:0-22:0"}}
```

```
% cat data.json | jq -s length
150346
```

```
% cat data.json | jq -s '.[0] | length'
14
```

```
% cat data.json | jq -s '.[0] | keys'
[
  "address",
  "attributes",
  "business_id",
  "categories",
  "city",
  "hours",
  "is_open",
  "latitude",
  "longitude",
  "name",
  "postal_code",
  "review_count",
  "stars",
  "state"
]
```

```
% cat data.json | jq -s '.[:3] | map(map_values(type))'
[
  {
    "business_id": "string",
    "name": "string",
    "address": "string",
    "city": "string",
    "state": "string",
    "postal_code": "string",
    "latitude": "number",
    "longitude": "number",
    "stars": "number",
    "review_count": "number",
    "is_open": "number",
    "attributes": "object",
    "categories": "string",
    "hours": "null"
  },
  {
    "business_id": "string",
    "name": "string",
    "address": "string",
    "city": "string",
    "state": "string",
    "postal_code": "string",
    "latitude": "number",
    "longitude": "number",
    "stars": "number",
    "review_count": "number",
    "is_open": "number",
    "attributes": "object",
    "categories": "string",
    "hours": "object"
  },
  {
    "business_id": "string",
    "name": "string",
    "address": "string",
    "city": "string",
    "state": "string",
    "postal_code": "string",
    "latitude": "number",
    "longitude": "number",
    "stars": "number",
    "review_count": "number",
    "is_open": "number",
    "attributes": "object",
    "categories": "string",
    "hours": "object"
  }
]
```

```
% cat data.json | jq -s '[.[].hours | type] | unique'
[
  "null",
  "object"
]
```

```
% cat data.json | jq -s '[.[].review_count] | min'
5
```

```
% cat data.json | jq -s '[.[].review_count] | max'
7568
```

```
% cat data.json | jq -s '[.[] | select(.review_count > 500)] | length'
1348
```

```
% cat data.json | jq -s '[.[].is_open] | unique' 
[
  0,
  1
]
```

```
% cat data.json | jq -s '[.[] | select(.review_count > 500) | select(.is_open == 1)] | length'
1224
```

```
% cat data.json | jq -s 'group_by(.state) | map({"state":.[0].state, "num":length}) | .[:5]'
[
  {
    "state": "AB",
    "num": 5573
  },
  {
    "state": "AZ",
    "num": 9912
  },
  {
    "state": "CA",
    "num": 5203
  },
  {
    "state": "CO",
    "num": 3
  },
  {
    "state": "DE",
    "num": 2265
  }
]
```

```
% cat data.json | jq -s 'group_by(.state) | map({"state":.[0].state, "num":length}) | sort_by(.num) | reverse | .[:5]'
[
  {
    "state": "PA",
    "num": 34039
  },
  {
    "state": "FL",
    "num": 26330
  },
  {
    "state": "TN",
    "num": 12056
  },
  {
    "state": "IN",
    "num": 11247
  },
  {
    "state": "MO",
    "num": 10913
  }
]
```

```
% cat data.json | jq -s '[.[].categories | type] | unique'
[
  "null",
  "string"
]
```

```
% cat data.json | jq -s 'map(select(.categories | type == "string")) | map(.categories | split(", ")) | flatten | unique | length'
1311
```

```
% cat data.json | jq -s 'map(select(.categories | type == "string")) | map(.categories | split(", ")) | flatten | unique | .[:10]'
[
  "& Probates",
  "3D Printing",
  "ATV Rentals/Tours",
  "Acai Bowls",
  "Accessories",
  "Accountants",
  "Acne Treatment",
  "Active Life",
  "Acupuncture",
  "Addiction Medicine"
]
```

```
% cat data.json | jq -s 'map(select(.categories | type == "string")) | map(.categories | split(", ")) | map(index("Doctors") != null) | map(select(. == true)) | length'
3763
```

```
% cat data.json | jq -s 'map(select(.categories | type == "string")) | [.[].categories] | map(contains("Doctors")) | map(select(. == true)) | length'
3763
```

