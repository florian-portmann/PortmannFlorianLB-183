# Handlungsziel 4
Ich werde hier auf Human Factor eingehen. Human Factor sagt, dass eines der grössten Sicherheitsprobelmen der Mensch selber ist. Wenn sich ein User zum Beispiel auf irgendwelchen unvertrauenswürgigen Webseiten einloggt, könnte der Besitzer dieses Passwort und Username speichern. Leider verwenden viele Menschen die gleichen Passwörter und Benutzernamen auf verschieden Webseiten. So könnte sich der Angreiffer je nachdem auf verschiedensten Webseiten einloggen, weil er die Logindaten gesepeichert hat. 

## Sicherheitsrelevante Aspekte bei Entwurf
Eine Möglichkeit um sich vor Human-Facotor-Gefahren zu schützen wäre, dass man das alte Passwort eingeben muss, wenn man sein Passwort zurücksetzen möchte. 
Ich möchte dies implementieren und zusätzlich soll das Passwort auch noch validiert werden. 


## Sicherheitsrelevante Aspekte bei Implementierung 
### Passwort ändern:
```csharp
public ActionResult PasswordUpdate(PasswordUpdateDto request)
        {
            if (request == null)
            {
                return BadRequest("No request body");
            }

            var user = _context.Users.Find(request.UserId);
            if (user == null)
            {
                return NotFound(string.Format("User {0} not found", request.UserId));
            }

            if (user.Password != MD5Helper.ComputeMD5Hash(request.OldPassword))
            {
                return Unauthorized("Old password wrong");
            }

            string passwordValidation = validateNewPasswort(request.NewPassword);
            if (passwordValidation != "")
            {
                return BadRequest(passwordValidation);
            }

            user.IsAdmin = request.IsAdmin;
            user.Password = MD5Helper.ComputeMD5Hash(request.NewPassword);

            _context.Users.Update(user);
            _context.SaveChanges();

            return Ok("success");
        }
```

### Validierung des neuen Passwort:
```csharp
 private string validateNewPasswort(string newPassword)
        {
            string patternSmall = "[a-zäöü]";
            Regex regexSmall = new Regex(patternSmall);
            bool hasSmallLetter = regexSmall.Match(newPassword).Success;

            string patternCapital = "[A-ZÄÖÜ]";
            Regex regexCapital = new Regex(patternCapital);
            bool hasCapitalLetter = regexCapital.Match(newPassword).Success;

            string patternNumber = "[0-9]";
            Regex regexNumber = new Regex(patternNumber);
            bool hasNumber = regexNumber.Match(newPassword).Success;

            List<string> result = new List<string>();
            if (!hasSmallLetter)
            {
                result.Add("keinen Kleinbuchstaben");
            }
            if (!hasCapitalLetter)
            {
                result.Add("keinen Grossbuchstaben");
            }
            if (!hasNumber)
            {
                result.Add("keine Zahl");
            }

            if (result.Count > 0)
            {
                return "Das Passwort beinhaltet " + string.Join(", ", result);
            }
            return "";
        }

```


## Sicherheitsrelevante Aspekte bei Inbetriebnahme
Wenn der User jetzt sein Passwort ändern will, muss er zuerst sein altes Passwort eingeben. Danach kann er sein neues Passwort wählen und das Passwort wird validiert. 

## Handlungszielvalidierung
Ich habe duch meinen Code und eine Beschreibung gezeigt, dass ich die Handlungsziele erfüllt habe. 
