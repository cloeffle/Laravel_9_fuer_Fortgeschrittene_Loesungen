**Übung 14:** Die Logs sollen ihr Passwort zurücksetzen

Wenn du in deiner .env die Konstante MAIL_MAILER=log setzt und den MAIL_LOG_CHANNEL=stack hinzufügst, kannst du die Funktion des Passwortzurücksetzens schon einmal ausprobieren. Die E-Mail zum Zurücksetzen des Passworts wird dann in deine Log-Datei geschrieben.

**Lösung:**

1. 

im Ordner config

mail.php öffnen:

hier ist default smtp eingestellt:

```

'default' => env('MAIL_MAILER', 'smtp'),

```

2.
Die .env Datei öffnen:

hier ist das PROBLEM ;-) : damit kann man nichts mailen!
```
MAIL_MAILER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=null
MAIL_FROM_NAME="${APP_NAME}"
``` 

```
// aendern
MAIL_MAILER=log
MAIL_LOG_CHANNEL=stack
MAIL_HOST=127.0.0.1
MAIL_PORT=2525
MAIL_USERNAME=advanced-laravel
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=null
MAIL_FROM_NAME="${APP_NAME}"

```

**Test:**

Du musst einen registrierten User im System haben, dessen email-Adresse Du kennst!

Im Login Dialog "Forgot Your Password?" auswählen:

http://advanced-laravel.test/password/reset


In der Log-Datei findet sich die Mail zu Passwort Reset, aber nur hier,
es wird keine Mail über den Email-Provider versendet!


