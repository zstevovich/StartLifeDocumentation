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

| Par.         | Type          |        Required        | Description                                                                                                                                                                                                                                      |
|:-------------|:--------------|:----------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `requestid`  | `string[50]`  |   :heavy_check_mark:   | requestid je identifikator request-a koji predstavlja izvor transakcije, requestid je tipa string koji se reprezentuje isključivo u numeričkom nizu karaktera (123654566663144888...) mora biti jedinstven(unique), ovaj parametar generiše kasa |
| `command`    | `enum`        |          [x]           | LYT_GETPOINTS                                                                                                                                                                                                                                    |
| `chainid`    | `integer[16]` |          [x]           | jedinstveni broj klijenta iz priloga A                                                                                                                                                                                                           |
| `shopid`     | `integer[32]` |          [ ]           | šifra objekta u kom je generisan request                                                                                                                                                                                                         |
| `shopname`   | `string[100]` |          [ ]           | naziv objekta u kom je generisan request                                                                                                                                                                                                         |
| `posno`      | `integer[32]` |          [x]           | broj kase na kojoj je generisan request                                                                                                                                                                                                          |
| `parameters` | `child`       |          [x]           |    
| Par.         | Type          |        Required        | Description                                                                                                                                                                                                                                      |
| `cardno`     | `string`      |          [x]           | broj očitane kartice                                                                                                                                                                                                                             |
                                                                                                                                                                                                 |

### Response

