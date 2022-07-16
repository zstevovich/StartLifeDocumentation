# LYT_GETPOINTS
#### Zahtev za komandu LYT_GETPOINTS
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