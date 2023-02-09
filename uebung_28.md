**Übung 28:** Dein erstes Event in deiner neuen Imbissbude

Lass uns doch mal das Beispiel mit der fiktiven Imbissbude in Laravel nachspielen.

1. Erstelle ein Event mit dem Namen OrderCompleted 

2. und zwei Listener mit den Namen PrepareCurrywurst und GenerateInvoice. 

Du kannst das Event mit den Listenern explizit im EventServiceProvider registrieren oder automatisch registrieren lassen.

**Lösung:**



1.
Einen neuen Event erstellen:

```
php artisan make:event OrderCompleted
```

Events etstehen im Ordner: app/Events

2.
Einen neuen Listener erstellen:

```
php artisan make:listener PrepareCurrywurst -e OrderCompleted
```
// statt     :  -e 
// geht auch :  --event

Listener entsehen im Ordner: app\Listeners

3.
Noch einen Listener erstellen:

```
php artisan make:listener GenerateInvoice -e OrderCompleted
```

4.
In app\Providers  EventServiceProvider.php öffnen:

Am Ende der Klasse eine neue Methode einfuegen:
 
``` 
 public function shouldDiscoverEvents(){
	return true;
 }
``` 

**Test:**

Die Events und zugeordnete Listener anzeigen lassen:

```
art event:list
```

```
+-----------------------------------+-------------------------------------------------------------+
| Event                             | Listeners                                                   |
+-----------------------------------+-------------------------------------------------------------+
| App\Events\OrderCompleted         | App\Listeners\GenerateInvoice@handle                        |
|                                   | App\Listeners\PrepareCurrywurst@handle                      |
|                                   |                                                             |
| Illuminate\Auth\Events\Registered | Illuminate\Auth\Listeners\SendEmailVerificationNotification |
+-----------------------------------+-------------------------------------------------------------+

```

