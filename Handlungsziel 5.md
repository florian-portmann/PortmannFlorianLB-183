# Handlungsziel 5
## Informationen für Auditing und Logging generieren.

### Konfuguration von Logger:
```csharp
builder.Host.ConfigureLogging(logging =>
{
    logging.ClearProviders();
    logging.AddConsole();
    logging.AddDebug();
});

```


### Logger anwenden:
 ```csharp

if (user == null)
            {
                _logger.LogWarning($"login failed for user '{request.Username}'");
                return Unauthorized("login failed");
            }

            _logger.LogInformation($"login successful for user '{request.Username}'");
            return Ok(CreateToken(user));
```

### Auditing:

Auditing bezeichnet die systematische Überwachung und Analyse von Aktivitäten innerhalb eines Systems oder einer Organisation, um die Einhaltung von Standards, Sicherheitsrichtlinien und gesetzlichen Vorschriften sicherzustellen. Es dient der Überprüfung, Überwachung und Dokumentation von Aktivitäten, um Compliance zu gewährleisten und potenzielle Risiken zu identifizieren.