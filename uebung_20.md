**Übung 20:** Versende deine vorhandenen Mails an ein reales E-Mail Postfach

Sende deine E-Mail mit Blade Template, 
eingebundenem Bild und einer reinen Textversion an dein Mailtrap-Postfach.

**Lösung:**

Entweder einen eigenen Provider oder mailtrap konfigurieren:

Mailtrap:
Damit wir die E-Mail auch versenden können, benötigen wir eine SMTP-E-Mail zum Versenden der Mails. Wir verwenden Mailtrap. Mailtrap ist ein kostenloses SMTP-Postfach. Registriere dich bei Mailtrap unter folgendem Link: https://mailtrap.io/register/signup?ref=header. Du kannst Github zum Anmelden verwenden. Nachdem du dich angemeldest hast, klicke auf die demo inbox. Kopiere dann die Zugangsdaten in die entsprechenden Felder der .env und verwende den Mailer smtp. Wenn du dies getan hast, kannst du den Mailer smtp zum Versenden deiner E-Mails verwenden. Eine versendete E-Mail landet dann in deinem demo-Postfach bei Mailtrap. Beachte bitte, dass du nur den kostenfreien Plan von Mailtrap verwendest. Es sind auch kostenpflichtige Varianten verfügbar, die wir aber nicht benötigen. 
In der .env Datei:



mailtrap konfigurieren:

.env-Datei:

```
ist:
MAIL_MAILER=log
MAIL_LOG_CHANNEL=jens
MAIL_HOST=127.0.0.1
MAIL_PORT=2525
MAIL_USERNAME=advanced-laravel
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=null
MAIL_FROM_NAME="${APP_NAME}"

soll:

MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=ef167XXXX4535a
MAIL_PASSWORD=c52cXXXX6e8112
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=ich@home.de
MAIL_FROM_NAME="${APP_NAME}"

```

**Test:**

alle Mail nochmal senden!

http://advanced-laravel.test/ansicht_in_logs

