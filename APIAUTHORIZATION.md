# Web Service autorizacija:

“Hash” za autorizaciju je base64 kodirana verzija heširanog stringa koji se generiše saSHA-512 algoritmom. 
Da biste generisali heš za autorizaciju, treba dodati sledeće vrednosti parametara i kreirati string navedenim 
redosledom korišćenjem "|" kao separator.<br/><br/>
LYT_SETPOINTS string = chainid + | + billno + | + amount + | + requestId + | +apiKey <br/>LYT_GETPOINTS string = chainid + | + requestId + | + apiKey
<br/><br/>Sada kriptujte stringsaSHA-512 algoritmom I konvertujte kriptovanu vrednost u base64format–dodatno šifrovanje. 
Dobijena vrednost predstavlja potpis koji se šalje u "header"-u zahteva/requesta. <br/><br/>
Identifikator zahteva (requestId) mora biti "unique". Početna 4 bajta predstavljaju jedinstveni broj klijenta(chainId) 
koji će vam biti dodeljen i dostavljen uz kredencijale. Primer kreiranja hash-a za komandu LYT_SETPOINT je prikazan ispod, 
za LYT_GETPOINT je identičan postupak sa odgovarajućim hash-om koji je već opisan u tekstu iznad:

### Primer C#
```csharp
using System.Security.Cryptography;
using System.Text;

public String MakeSignatureHash()
    { 
        var hashString = "2632|569856631|25600.50|263231912051259417|TUY256XZ";
        var bytes = Encoding.UTF8.GetBytes(hashString);
        using var hash=SHA512.Create();
        var hashedInputBytes=hash.ComputeHash(bytes);
        var hashedInputStringBuilder = newStringBuilder(128);
        foreach(var binhashedInputBytes)
            {
                hashedInputStringBuilder.Append(b.ToString("x2"));
            }
        var plainTextBytes = Encoding.UTF8.GetBytes(hashedInputStringBuilder.ToString());
        return Convert.ToBase64String(plainTextBytes);
    }
```
### Primer JAVA
```java
import java.math.BigInteger;
import java.security.MessageDigest;
import java.util.Base64;
import java.security.NoSuchAlgorithmException;

    public StringmakeSignatureHash() throwsNoSuchAlgorithmException {
        String hashString = "2632|569856631|25600.50|263231912051259417|TUY256XZ";
        MessageDigest messageDigest = MessageDigest.getInstance("SHA-512");
        messageDigest.update(hashString.getBytes());
        byte[] digest = messageDigest.digest();
        String digestHex = String.format("%0128x", new BigInteger(1, digest));
        return new String(Base64.getEncoder().encode(digestHex.getBytes()));
        }

```
#### U header-u svakog zahteva se prosleđuje hash kao parametar signature:

```curl
curl --location --request POST /api/life/req --header 
'signature:MjU4ZjFjODRhZTAxMTcyYjBkNDBlNjhmMjljODllMDUwZjI0NWUwZDJhZWU4ZmY1NjViMWZjZmQ5YmJmMTBmOTM1ZWM4MDA4NGZlYjc3Nzcx
YWI0NjVlMzEwZmVhYzUxZjY0NGFmNDZmNGIzOTU3OWZiOGQzZWMxZjk2YTc2YWE='
```
