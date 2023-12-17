# Handlungsziel 3
## Mechanismen für die Authentifizierung umsetzen

```csharp
private string CreateToken(User user)
        {
            string issuer = _configuration.GetSection("Jwt:Issuer").Value!;
            string audience = _configuration.GetSection("Jwt:Audience").Value!;

            List<Claim> claims = new List<Claim> {
                    new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
                    new Claim(JwtRegisteredClaimNames.NameId, user.Id.ToString()),
                    new Claim(JwtRegisteredClaimNames.UniqueName, user.Username),
                    new Claim(ClaimTypes.Role,  (user.IsAdmin ? "admin" : "user"))
            };

            string base64Key = _configuration.GetSection("Jwt:Key").Value!;
            SymmetricSecurityKey securityKey = new SymmetricSecurityKey(Convert.FromBase64String(base64Key));

            SigningCredentials credentials = new SigningCredentials(
                    securityKey,
                    SecurityAlgorithms.HmacSha512Signature);

            JwtSecurityToken token = new JwtSecurityToken(
                issuer: issuer,
                audience: audience,
                claims: claims,
                notBefore: DateTime.Now,
                expires: DateTime.Now.AddDays(1),
                signingCredentials: credentials
             );

            return new JwtSecurityTokenHandler().WriteToken(token);
        }
```
Die `CreateToken(User user)`-Methode ist verantwortlich für die Generierung eines JWT-Tokens (JSON Web Token), der im Rahmen der Authentifizierung von Benutzern verwendet wird. Ihre Funktionalität gliedert sich in verschiedene Schritte:

1. **Claims-Erstellung:** In dieser Methode werden Ansprüche definiert, die im JWT-Token enthalten sein sollen. Diese Claims enthalten typischerweise Informationen wie die Benutzer-ID (`user.Id`), den Benutzernamen (`user.Username`) und die Rolle des Benutzers (beispielsweise `admin` oder `user`, je nachdem, ob der Benutzer Administratorrechte besitzt).

2. **Token-Konfiguration:** Es werden Parameter für den JWT-Token festgelegt, darunter der Aussteller (`issuer`), der Empfänger (`audience`), die Gültigkeitsdauer (`notBefore` und `expires`) sowie die Signierungsanmeldeinformationen (`credentials`).

3. **Erstellung des JWT-Tokens:** Basierend auf den definierten Ansprüchen, der Konfiguration und den Signierungsanmeldeinformationen wird der JWT-Token erstellt.

4. **Rückgabe des Tokens:** Schließlich wird der generierte Token als string zurückgegeben. Dieser Token repräsentiert den JWT-Token, der üblicherweise an den Client gesendet wird, nachdem ein Benutzer erfolgreich authentifiziert wurde.



## Mechanismen für die Autorisierung umsetzen
Autorisierung ist die Zuweisung von Benutzern zu ihren passenden Rollen. 
In diesem Beispiel wären das Benutzer und Administrator. 

Die Rollen sollen verschiedene Fähigkeiten haben. Der Administrator soll alle Fähigkeiten haben, während der Benutzer nur einige bestimmte Fähigkeiten haben soll. 
