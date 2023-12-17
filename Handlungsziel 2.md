# Handlungsziel 2

## Sicherheitslücken und ihre Ursachen in einer Applikation erkennen

```csharp
public ActionResult<User> Login(LoginDto request)
        {
            if (request == null || request.Username.IsNullOrEmpty() || request.Password.IsNullOrEmpty())
            {
                return BadRequest();
            }

            string sql = string.Format("SELECT * FROM Users WHERE username = '{0}' AND password = '{1}'", 
                request.Username, 
                MD5Helper.ComputeMD5Hash(request.Password));

            User? user= _context.Users.FromSqlRaw(sql).FirstOrDefault();
            if (user == null)
            {
                return Unauthorized("login failed");
            }
            return Ok(user);
        }
```

Die Sicherheitslücke liegt in der unsicheren Verarbeitung der Benutzereingaben in der SQL-Abfrage des Login-Prozesses. Durch direkte Einfügung von manipulierten Werten in die SQL-Abfrage, insbesondere im string.Format-Teil, könnten Angreifer Schadcode einschleusen und die Abfrage so manipulieren, dass sie sich ohne korrektes Passwort authentifizieren können. Diese Schwachstelle öffnet die Tür für sogenannte SQL-Injection-Angriffe, bei denen Angreifer die Datenbank manipulieren oder unerlaubten Zugriff erlangen können.

## Gegenmassnahmen vorschlagen und implementieren

```csharp

public ActionResult<User> Login(LoginDto request)
        {
            if (request == null || request.Username.IsNullOrEmpty() || request.Password.IsNullOrEmpty())
            {
                return BadRequest();
            }

            string username = request.Username;
            string passwordHash = MD5Helper.ComputeMD5Hash(request.Password);


            User? user = _context.Users
                .Where(u => u.Username == username)
                .Where(u => u.Password == passwordHash)
                .FirstOrDefault();


            if (user == null)
            {
                return Unauthorized("login failed");
            }
            return Ok(user);
        }
```

Dieser aktualisierte Code schließt die Sicherheitslücke durch SQL-Injection auf folgende Weise:

1. **Verwendung von Parameterisierung:**
   - Die Benutzereingaben werden außerhalb der SQL-Abfrage als Variablen gespeichert: `string username = request.Username;` und `string passwordHash = MD5Helper.ComputeMD5Hash(request.Password);`.
   - Anstatt die Benutzereingaben direkt in die SQL-Abfrage einzufügen, werden sie als Parameter behandelt und in einer parameterisierten Abfrage verwendet.

2. **Verwendung von ORM (Object-Relational Mapping):**
   - Die SQL-Abfrage wurde durch eine LINQ-Abfrage ersetzt (`_context.Users.Where(...)`), die ORM-Funktionalität nutzt.
   - Durch die Verwendung von LINQ wird die Datenbankabfrage sicherer gestaltet, da sie sicherstellt, dass Benutzereingaben nicht direkt in die Abfrage eingefügt werden.
   - Die Abfrage überprüft den Benutzernamen und den Passwort-Hash in der Datenbank, ohne direkte Einfügungen von Benutzereingaben in die Abfrage zu verwenden.


