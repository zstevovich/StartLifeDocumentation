1. **LYT_GETPOINTS** je nekritična komanda koja ne zahteva nikakve radnje u slučaju da nije uspešno 
obrađena. Ako kasa ne dobije odgovor servera to ne znači da nije moguće poslati komandu za upis transakcije.
Ova komanda treba da informiše kasu o statusu kartice/polise kupca kako bi eventualno prekinula dalje operacije
u situacijama kada je kartica u nekom blokirajućem statusu ali je to ne ograničava da pošalje zahtev za 
upis transakcije - LYT_SETPOINTS u situaciji kada nema odgovora servera.

2. **LYT_SETPOINTS** je kritična komanda. Kupac treba da dobije svoje benefite ova komanda povećava vrednost polise.
Ukoliko ne dobije odgovor servera usled prekida komunikacije kasa može da pokuša ponovo da upiše transakciju i to može da 
radi diskretno dok god ne dobije odgovor servera o statusu zahtevane transakcije.

- `Datum i vreme koji se razmenjuju life servisima su u formatu dd.MM.yyyy hh:mm:ss i tipa su string`
- `Svi novčani iznosi se interpretiraju kao string, format 2525.50, 2500.55...sa dve decimale "####.##"`
- `Svi boolean parametri vraćaju true/false`

# Komande
### **LYT_GETPOINTS**

#### xml endpoint
```http
POST /api/Life/Req
```
#### json endpoint
```http
POST /api/Life/ReqJson
```
#### body

| Par.                 | Type               | Required | Description                                                                                                                                                                                                                                      |
|:---------------------|:-------------------|:--------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `requestid`          | `string[50]`       | &check;  | requestid je identifikator request-a koji predstavlja izvor transakcije, requestid je tipa string koji se reprezentuje isključivo u numeričkom nizu karaktera (123654566663144888...) mora biti jedinstven(unique), ovaj parametar generiše kasa |
| `command`            | `enum`             | &check;  | LYT_GETPOINTS                                                                                                                                                                                                                                    |
| `chainid`            | `integer[16]`      | &check;  | jedinstveni broj klijenta iz priloga A                                                                                                                                                                                                           |
| `shopid`             | `integer[32]`      | &cross;  | šifra objekta u kom je generisan request                                                                                                                                                                                                         |
| `shopname`           | `string[100]`      | &cross;  | naziv objekta u kom je generisan request                                                                                                                                                                                                         |
| `posno`              | `integer[32]`      | &check;  | broj kase na kojoj je generisan request                                                                                                                                                                                                          |
| `parameters`         | `childobject`      |          | child bjekat                                                                                                                                                                                                                                     |
| `parametsers.cardno` | `string`           | &check;  | broj očitane kartice                                                                                                                                                                                                                             |

### Response

| Par.                  | Type          | Description                                              |
|-----------------------|---------------|----------------------------------------------------------|
| `requestid`           | `string`      | Prosleđeni request id                                    |
| `status`              | `childobject` |                                                          |
| `status.code`         | `integer`     | Kod odgovora servera                                     |
| `status.description`  | `string`      | Opis odgovora servera                                    |
| `data`                | `childobject` |                                                          |
| `data.yearsinsurance` | `integer`     | Godina osiguranja - informativni podatak                 |
| `data.active`         | `boolean`     | Status polise / kartice, da li je aktivna ili ne         |
| `data.amountmax`      | `string`      | Preostali dozvoljeni iznos za uplatu do godišnjeg limita | 

### **LYT_SETPOINTS**

#### xml endpoint
```http
POST /api/Life/Req
```
#### json endpoint
```http
POST /api/Life/ReqJson
```
#### body

| Par.                     | Type          | Required | Description                                                                                                                                                                                                                                      |
|:-------------------------|:--------------|:--------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `requestid`              | `string[50]`  | &check;  | requestid je identifikator request-a koji predstavlja izvor transakcije, requestid je tipa string koji se reprezentuje isključivo u numeričkom nizu karaktera (123654566663144888...) mora biti jedinstven(unique), ovaj parametar generiše kasa |
| `command`                | `enum`        | &check;  | LYT_SETPOINTS                                                                                                                                                                                                                                    |
| `chainid`                | `integer[16]` | &check;  | jedinstveni broj klijenta iz priloga A                                                                                                                                                                                                           |
| `shopid`                 | `integer[32]` | &cross;  | šifra objekta u kom je generisan request                                                                                                                                                                                                         |
| `shopname`               | `string[100]` | &cross;  | naziv objekta u kom je generisan request                                                                                                                                                                                                         |
| `posno`                  | `integer[32]` | &check;  | broj kase na kojoj je generisan request                                                                                                                                                                                                          |
| `parameters`             | `childobject` |          | child objekat                                                                                                                                                                                                                                    |
| `parametsers.cardno`     | `string`      | &check;  | broj očitane kartice                                                                                                                                                                                                                             |
| `parameters.cashierid`   | `integer[32]` | &cross;  | šifra prodavca koji je izvršio transakciju                                                                                                                                                                                                       |
| `parameters.cashiername` | `string[100]` | &cross;  | naziv prodavca koji je izvršio transakciju                                                                                                                                                                                                       |
| `parameters.currency`    | `string`      | &check;  | valuta u formatu ISO 4217 (RSD)                                                                                                                                                                                                                  |
| `parameters.datetime`    | `string`      | &check;  | vreme kada je izvršena transakcija u formatu `dd.MM.yyyy hh:mm:ss`                                                                                                                                                                               |
| `parameters.billno`      | `string`      | &check;  | broj računa (PFR brojač)                                                                                                                                                                                                                         |
| `parameters.totalamount` | `string`      | &check;  | iznos računa za obračun life iznosa/ benefita u formatu `####.##`                                                                                                                                                                                |
| `parameters.billtotal`   | `string`      | &check;  | ukupan iznos računa sa sa svim stavkama u formatu `####.##`                                                                                                                                                                                      |
| `parameters.roundamount` | `string`      | &cross;  | iznos koji je dodat kako bi se zaokružila vrednost računa u formatu `####.##`                                                                                                                                                                    |
| `parameters.addamount`   | `string`      | &cross;  | dodatni iznos koji kupac uplaćuje na svoju polisu u formatu `####.##`                                                                                                                                                                            |

### Response

| Par.                  | Type          | Description                                              |
|-----------------------|---------------|----------------------------------------------------------|
| `requestid`           | `string`      | Prosleđeni request id                                    |
| `status`              | `childobject` |                                                          |
| `status.code`         | `integer`     | Kod odgovora servera                                     |
| `status.description`  | `string`      | Opis odgovora servera                                    |
| `data`                | `childobject` |                                                          |
| `data.yearsinsurance` | `integer`     | Godina osiguranja - informativni podatak                 |
| `data.active`         | `boolean`     | Status polise / kartice, da li je aktivna ili ne         |
| `data.amountmax`      | `string`      | Preostali dozvoljeni iznos za uplatu do godišnjeg limita | 
| `data.lifeamount`     | `string`      | Iznos obračunat i dodeljen na polisu kupca               |

### **Autentifikacija / dobijanje apiKey-a**

#### xml endpoint
```http
POST /api/Life/GetApiKey
```
#### json endpoint
```http
POST /api/Life/GetApiKeyJson
```
#### body
| Par.        | Type         | Required | Description                                                                                                                                                                                                                                      |
|:------------|:-------------|:--------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `requestid` | `string[50]` | &check;  | requestid je identifikator request-a koji predstavlja izvor transakcije, requestid je tipa string koji se reprezentuje isključivo u numeričkom nizu karaktera (123654566663144888...) mora biti jedinstven(unique), ovaj parametar generiše kasa |
| `username`  | `string`     | &check;  | Korisničko ime iz priloga A                                                                                                                                                                                                                      |
| `password`  | `string`     | &check;  | šifra iz priloga A                                                                                                                                                                                                                               |                                                                                                                                                                          |

### Response

| Par.                 | Type          | Description                  |
|----------------------|---------------|------------------------------|
| `requestid`          | `string`      | Prosleđeni request id        |
| `status`             | `childobject` |                              |
| `status.code`        | `integer`     | Kod odgovora servera         |
| `status.description` | `string`      | Opis odgovora servera        |
| `data`               | `childobject` |                              |
| `data.apikey`        | `string`      | Ključ za kriptovanje zahteva |

# Kodovi odgovora api servera

| Code | Status                   |
|:----:|:-------------------------|
|  0   | `OK`                     |
|  1   | `PARTIALLY_APPROVED`     |
|  2   | `SYSTEM_ERROR`           |
|  3   | `DUPLICATE_TRANSACTION`  |
|  4   | `INVALID_DATA_ERROR`     |
|  5   | `ACCESS_ERROR`           |
|  6   | `OPTIMISTIC_CONCURRENCY` |
|  9   | `SETUP_ERROR`            |

# Svi scenariji odgovora servera

| Code | Description                                                                                                                                                                |
|:----:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  0   | `Transakcija je uspešno registrovana ili zahtev je uspešno obrađen`                                                                                                        |
|  4   | `Kartica sa ovim brojem nije aktivirana`                                                                                                                                   |
|  4   | `Kartica sa ovim brojem ne postoji`                                                                                                                                        |
|  2   | `Nehendlovane greške`                                                                                                                                                      |
|  4   | `Kartica sa ovim brojem nije više validna`                                                                                                                                 |
|  4   | `Kartica je već aktivirana!`                                                                                                                                               |
|  4   | `Iznos je veći od maksimalnog godišnjeg iznosa!`                                                                                                                           |
|  5   | `Sigurnosni potpis je neispravan`                                                                                                                                          |
|  9   | `Korisničko ime ili šifra su neispravni`                                                                                                                                   |
|  3   | `Transakcija je već registrovana(u data elementu su detalji registrovane transakcije)`                                                                                     |
|  6   | `Optimistična kontrola poklapanja-OKP(druga transakcija jepromenilastanjekartice)`                                                                                         |
|  1   | `Transakcija je parcijalno registrovana(u data elementu su detalji transakcije, parcijalna transakcija se javlja kada je iznos benefita veći od godinjeg limita amountmax` |
