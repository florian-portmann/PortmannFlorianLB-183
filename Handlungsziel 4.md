# Handlungsziel 4
Ich werde hier auf Human Factor eingehen. Human Factor sagt, dass eines der grössten Sicherheitsprobelmen der Mensch selber ist. Wenn sich ein User zum Beispiel auf irgendwelchen unvertrauenswürgigen Webseiten einloggt, könnte der Besitzer dieses Passwort und Username speichern. Leider verwenden viele Menschen die gleichen Passwörter und Benutzernamen auf verschieden Webseiten. So könnte sich der Angreiffer je nachdem auf verschiedensten Webseiten einloggen, weil er die Logindaten gesepeichert hat. 

## Sicherheitsrelevante Aspekte bei Entwurf
Eine Möglichkeit um sich vor Human-Facotor-Gefahren zu schützen wäre, dass man das alte Passwort eingeben muss, wenn man sein Passwort zurücksetzen möchte. 


### Sicherheitsrisiko beim Passwort-Reset ohne Überprüfung des alten Passworts

Ein Passwort-Reset ohne Überprüfung des alten Passworts birgt das Risiko eines unbefugten Zugriffs auf Konten. Wenn ein Benutzer das Passwort zurücksetzen kann, ohne das alte Passwort einzugeben, könnten Unbefugte möglicherweise das Zurücksetzungsverfahren manipulieren und unautorisierten Zugriff auf das Konto erlangen.

### Warum das Einbeziehen des alten Passworts wichtig ist

Das Einbeziehen des alten Passworts als Bestätigung beim Passwort-Reset stellt sicher, dass der Benutzer seine Identität authentifizieren muss, bevor Änderungen am Passwort vorgenommen werden. Dadurch wird das Risiko von unbefugtem Zugriff minimiert und die Sicherheit des Kontos verbessert.


## Sicherheitsrelevante Aspekte bei Implementierung 
### Passwort ändern

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



## Sicherheitsrelevante Aspekte bei Inbetriebnahme

