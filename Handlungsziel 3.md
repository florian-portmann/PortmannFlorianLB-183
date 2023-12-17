# Handlungsziel 3
## Mechanismen für die Authentifizierung umsetzen
private string CreateToken(User user)
csharp
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



## Mechanismen für die Autorisierung umsetzen
Autorisierung ist die Zuweisung von Benutzern zu ihren passenden Rollen. 
In diesem Beispiel wären das Benutzer und Administrator. 

Die Rollen sollen verschiedene Fähigkeiten haben. Der Administrator soll alle Fähigkeiten haben, während der Benutzer nur einige bestimmte Fähigkeiten haben soll. 
