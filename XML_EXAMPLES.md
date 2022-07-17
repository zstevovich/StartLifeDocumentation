# LYT_GETPOINTS
#### Zahtev za komandu LYT_GETPOINTS
```http
POST /api/Life/Req
```
```xml
<asmmreq xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>120835699953559648</requestid>
    <command>LYT_GETPOINTS</command>
    <chainid>1208</chainid>
    <shopid>1236</shopid>
    <shopname>optional</shopname>
    <posno>1203</posno>
    <parameters>
        <cardno>879281740738</cardno>
    </parameters>
</asmmreq>
```
#### Odgovor za komandu LYT_GETPOINTS
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>0</code>
        <description>Ispravan zahtev</description>
    </status>
    <data>
        <yearsinsurance>1</yearsinsurance>
        <active>true</active>
        <amountmax>95320.50</amountmax>
    </data>
</asmmres>
```
# LYT_SETPOINTS
#### Zahtev za komandu LYT_SETPOINTS
```http
POST /api/Life/Req
```
```xml
<asmmreq xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>120835699953559648</requestid>
    <command>LYT_SETPOINTS</command>
    <chainid>1208</chainid>
    <shopid>1236</shopid>
    <shopname>optional</shopname>
    <posno>1203</posno>
    <parameters>
        <cardno>879281740738</cardno>
        <cashierid>optional</cashierid>
        <cashiername>optional</cashiername>
        <currency>RSD</currency>
        <datetime>19.05.2022 15:36:25</datetime>
        <billno>XYZ569-XYZ56-39</billno>
        <billtotal>1500.00</billtotal>
        <totalamount>1250.50</totalamount>
    </parameters>
</asmmreq>
```
#### Odgovor za komandu LYT_SETPOINTS
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>0</code>
        <description>Ispravan zahtev</description>
    </status>
    <data>
        <lifeamount>256.50</lifeamount>
        <yearsinsurance>1</yearsinsurance>
        <active>true</active>
        <amountmax>95320.50</amountmax>
    </data>
</asmmres>
```
# Autentifikacija - izdavanje apiKey
#### Zahtev za izdavanje akiKey
```http
POST /api/Life/GetApiKey
```
```xml
<asmmreq xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <requestid>120835699953559648</requestid>
    <username>test</username>
    <password>test1</password>
</asmmreq>
```
#### Odgovor na zahtev
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>120835699953559648</requestid>
    <status>
        <code>0</code>
        <description>Ispravan zahtev</description>
    </status>
    <data>
        <apikey>JQ5BT3ReDS</apikey>
    </data>
</asmmres>
```
# Primeri odgovora sa različitim kodom - kodovi koji se razlikuju od koda 0
#### Upozorenje u vezi sa godišnjim limitom polise
Primer u kom je zbog prekoračenja godišnjeg limita obračunato 9 dinara, u parametru warning stoji u lifeamount 9 dinara
, a to je iznos koji je moguće obračunati do limita, takođe info za zaokruži roundamount -10 dinara što znači da kupcu treba
vratiti taj iznos jer je odbijen za uplatu zbog prekoračenja limita.
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>1</code>
        <description>Upozorenje u vezi sa godišnjim limitom polise</description>
    </status>
    <data>
        <lifeamount>9.00</lifeamount>
        <yearsinsurance>1</yearsinsurance>
        <active>true</active>
        <amountmax>0.00</amountmax>
    </data>
    <warning>
        <lifeamount>9</lifeamount>
        <roundamount>-10.00</roundamount>
        <addamount>-2.50</addamount>
    </warning>
</asmmres>
```
#### Nehendlovane greške
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>2</code>
        <description>Sistemska Greška</description>
    </status>
</asmmres>
```
#### Snapshot već registrovanje transakcije - dupla transakcija
Primer greške kada je poslat zahtev sa requestid - jem koji je već registrovan, sistem vraća snapshot tog requesta jer 
je transakcija već registrovana i obaveštava o tome, ovaj odgovor se može koristiti i kao mehanizam provere kada je došlo
do prekida komunikacije u momentu slanja requesta i kasa nije uspela da snimi odgovor servera
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>3</code>
        <description>Transakcija prema ovom zahtevu (requestid) je već registrovana</description>
    </status>
    <data>
        <lifeamount>256.50</lifeamount>
        <yearsinsurance>1</yearsinsurance>
        <active>true</active>
        <amountmax>95320.50</amountmax>
    </data>
</asmmres>
```
#### Validacione greške
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>4</code>
        <description>Neispravni podaci</description>
    </status>
    <data>
        <errors>
            <name>requestid</name>
            <message>request id je neispravan</message>
        </errors>
        <errors>
            <name>parameters.cardno</name>
            <message>Broj kartice mora imati 12 karaktera</message>
        </errors>
    </data>
</asmmres>
```
#### Neispravan potpis - hash
Greška koja obaveštava da nije dobro generisan hash, a koji se šalje kao varijabla signature u headeru u base64 formatu
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>5</code>
        <description>Neispravan potpis</description>
    </status>
</asmmres>
```
#### Neispravni kredencijali za dobijanje apiKey
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>9</code>
        <description>Korisnočko ime ili šifra nisu ispravni</description>
    </status>
</asmmres>
```
#### Optimistic concurrency greška
```xml
<asmmres xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <requestid>1236545585888</requestid>
    <status>
        <code>6</code>
        <description>Druga transakcija je u toku, pokušajte ponovo da pošaljete zahtev</description>
    </status>
</asmmres>
```