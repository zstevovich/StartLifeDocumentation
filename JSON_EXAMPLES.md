# LYT_GETPOINTS
#### Zahtev za komandu LYT_GETPOINTS
```http
POST /api/Life/ReqJson
```
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
```http
POST /api/Life/ReqJson
```
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
# Autentifikacija - izdavanje apiKey
#### Zahtev za izdavanje akiKey
```http
POST /api/Life/GetApiKeyJson
```
```json
{
  "requestid": "120835699953559648",
  "username": "test1",
  "password": "test1235"
}
```
#### Odgovor na zahtev
```json
{
  "requestid": "120835699953559648",
  "status": {
    "code": 0,
    "description": "Ispravan zahtev"
  },
  "data": {
    "apikey": "JQ5BT3ReDS"
  }
}
```
# Primeri odgovora sa različitim kodom - kodovi koji se razlikuju od koda 0
#### Upozorenje u vezi sa godišnjim limitom polise
Primer u kom je zbog prekoračenja godišnjeg limita obračunato 9 dinara, u parametru warning stoji u lifeamount 9 dinara
, a to je iznos koji je moguće obračunati do limita, takođe info za zaokruži roundamount -10 dinara što znači da kupcu treba
vratiti taj iznos jer je odbijen za uplatu zbog prekoračenja limita.
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 1,
    "description": "Upozorenje u vezi sa godišnjim limitom polise"
  },
  "data": {
    "lifeamount": "9.00",
    "yearsinsurance": 1,
    "active": true,
    "amountmax": "0.00"
  },
  "warning": {
    "lifeamount": "9.00",
    "roundamount": "-10.00",
    "addamount": "-2.50"
  }
}
```
#### Nehendlovane greške
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 2,
    "description": "Sistemska Greška"
  },
  "data": {}
}
```
#### Snapshot već registrovanje transakcije - dupla transakcija
Primer greške kada je poslat zahtev sa requestid - jem koji je već registrovan, sistem vraća snapshot tog requesta jer
je transakcija već registrovana i obaveštava o tome, ovaj odgovor se može koristiti i kao mehanizam provere kada je došlo
do prekida komunikacije u momentu slanja requesta i kasa nije uspela da snimi odgovor servera
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 3,
    "description": "Transakcija prema ovom zahtevu (requestid) je već registrovana"},
  "data": {
    "lifeamount": "256.50",
    "yearsinsurance": 1,
    "active": true,
    "amountmax": "95320.50"
  }
}
```
#### Validacione greške
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 4,
    "description": "Kartica sa ovim brojem nije aktivirana"
  },
  "data": {
    "errors": [
      {
      "name": "parameters.cardno",
      "message": "Kartica sa ovim brojem nije aktivirana"
      }
    ]
  }
}
```
#### Neispravan potpis - hash
Greška koja obaveštava da nije dobro generisan hash, a koji se šalje kao varijabla signature u headeru u base64 formatu
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 5,
    "description": "Neispravan potpis"
  },
  "data": {}
}
```
#### Neispravni kredencijali za dobijanje apiKey
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 9,
    "description": "Korisnočko ime ili šifra nisu ispravni"
  },
  "data": {}
}
```
#### Optimistic concurrency greška
```json
{
  "requestid": "1236545585888",
  "status": {
    "code": 6,
    "description": "Druga transakcija je u toku, pokušajte ponovo da pošaljete zahtev"
  },
  "data": {}
}
```