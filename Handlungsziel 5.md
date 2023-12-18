# Handlungsziel 5
## Informationen für Auditing und Logging generieren.


cshap```
builder.Host.ConfigureLogging(logging =>
{
    logging.ClearProviders();
    logging.AddConsole(); // Console Output
    logging.AddDebug(); // Debugging Console Output
});

``` 
