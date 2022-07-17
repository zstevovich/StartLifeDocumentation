# LYT_GETPOINTS
#### Zahtev za komandu LYT_GETPOINTS
```json
{
  "requestid": "120835699953559648",
  "command": "LYT_GETPOINTS",
  "chainid": 1208,
  "shopid": 1236,
  "shopname": "optional",
  "posno": 1203,
  "parameters": {
    "cardno": "879281740738"
  }
}
```
#### Odgovor za komandu LYT_GETPOINTS
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 0,
    "description": "Ispravan zahtev"
  },
  "data": {
    "yearsinsurance": 1,
    "active": true,
    "amountmax": "95320.50"
  }
}
```
# LYT_SETPOINTS
#### Zahtev za komandu LYT_SETPOINTS
```json
{
  "requestid": "120835699953559648",
  "command": "LYT_SETPOINTS",
  "chainid": 1208,
  "shopid": 1236,
  "shopname": "optional",
  "posno": 1203,
  "parameters": {
    "cardno": "879281740738",
    "cashierid": "optional",
    "cashiername": "optional",
    "currency": "RSD",
    "datetime": "19.05.2022 15:36:25",
    "billno": "XYZ569-XYZ56-39",
    "billtotal": "1500.00",
    "totalamount": "1250.50"
  }
}
```
#### Odgovor za komandu LYT_SETPOINTS
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 0,
    "description": "Ispravan zahtev"
  },
  "data": {
    "lifeamount": "256.50",
    "yearsinsurance": 1,
    "active": true,
    "amountmax": "95320.50"
  }
}
```