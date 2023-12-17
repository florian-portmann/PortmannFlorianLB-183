# PortmannFlorianLB-183

## Einleitung
In diesem Portfolioeintrag werde ich sehr genau aufzeigen, wie man sich vor Hackingangriffen schützen kann. Dies werde ich anhand des Codes der Aufträgen vom Modul 183 tun.

## Handlungsziel 1
### Aktuelle Bedrohungen erkennen und erläutern
Broken Access Control (A01):
Erkennung der Bedrohung:

Bitte überwachen Sie Zugriffsprotokolle, um unerwartete oder ungewöhnliche Zugriffsversuche auf sensible Daten oder Funktionen zu identifizieren.
Überprüfen Sie bitte die Benutzerberechtigungen, um sicherzustellen, dass nur autorisierte Benutzer auf bestimmte Ressourcen zugreifen können und dass die Zugriffsrechte angemessen eingerichtet sind.

Erläuterung:

Broken Access Control bezieht sich auf Schwächen in der Zugriffskontrolle, die es Angreifern ermöglichen, auf Bereiche oder Funktionen zuzugreifen, für die sie keine Berechtigung haben sollten. Das kann zu Datenlecks, unautorisierten Änderungen oder sogar zur Übernahme des Systems führen. Diese Schwachstelle ist oft durch unzureichende Validierung von Zugriffsanfragen oder mangelhafte Berechtigungsprüfungen bedingt.

Injection (A03):
Erkennung der Bedrohung:

Bitte überprüfen Sie alle Benutzereingaben und Parameter, um sicherzustellen, dass sie keine unerlaubten Befehle oder Skripte enthalten.
Nutzen Sie bitte Tools wie eine Web Application Firewall, um schädliche Injektionen wie SQL-Injections oder Cross-Site Scripting (XSS) zu erkennen und zu blockieren.
Erläuterung:
Injection-Angriffe umfassen das Einschleusen von schädlichem Code oder Befehlen in Anwendungen oder Systeme. Diese Angriffe können zu Datenmanipulationen, Datenlecks oder sogar zur Übernahme des Systems führen. Die häufigsten Typen sind SQL-Injections und Cross-Site Scripting (XSS), bei denen Angreifer Code einschleusen, um Datenbanken zu manipulieren oder Benutzer dazu zu bringen, unerwünschte Aktionen auszuführen.

Security Misconfiguration (A05):
Erkennung der Bedrohung:

Bitte verwenden Sie Tools oder Scanner, um Sicherheitskonfigurationsfehler in Systemen oder Anwendungen zu erkennen.
Implementieren Sie bitte klare Sicherheitsrichtlinien und überwachen Sie regelmäßig die Konfiguration von Systemen, um sicherzustellen, dass sie den Standards entsprechen.
Erläuterung:
Sicherheitskonfigurationsfehler treten auf, wenn Systeme, Plattformen oder Anwendungen nicht ordnungsgemäß konfiguriert sind. Dies kann dazu führen, dass sensible Daten ungeschützt sind, unerwünschte Zugriffe ermöglicht werden oder dass Sicherheitsmechanismen nicht richtig funktionieren. Solche Fehler können durch unsichere Standardeinstellungen, nicht entfernte Testkonfigurationen oder fehlende Updates entstehen.
