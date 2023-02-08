**Übung 24:** Dein erster Arbeiter

Es wird Zeit, dass sich ein Arbeiter um deine Aufgabenliste kümmert. 

Starte einen Queue Worker. 

Sobald du den Befehl ausgeführt hast, siehst du, dass die entsprechenden Logs geschrieben wurden.


**Lösung:**


1.
predis installieren!

A flexible and feature-complete Redis client for PHP 7.2 and newer.

```

composer require predis/predis

oder:

composer require predis/predis:*

oder:

composer require predis/predis:* --ignore-platform-reqs

```

2.
In der .env  von sync auf redis umstellen!

Wenn man nicht umstellt, wird alles sofort geloggt!!!! 

```
# alte Einstellung
#QUEUE_CONNECTION=sync 
# neue Einstellung
QUEUE_CONNECTION=redis 

```

3.
in der web.php eine Route erstellen:

```

// uebung_24
Route::get('/log_uebung_24',function(){
	
  Log::alert('Aufgabe ausgeführt!');
	
	dispatch(function(){
		Log::alert('Aufgabe in der Queue ausgeführt!');
	});
});

```

4.
Die Log-Datei(en) leeren!

5.
Den  Queue-Worjer starten!

```
php artisan queue:work
```

**Test:**

http://http://advanced-laravel.test/log_uebung_24

Im Log nachschauen!

```
[202x-mm-dd 19:23:58] local.ALERT: Aufgabe ausgeführt!  
[202x-mm-dd 19:23:58] local.ALERT: Aufgabe ausgeführt!  
[202x-mm-dd 19:24:22] local.ALERT: Aufgabe ausgeführt!  
[202x-mm-dd 19:24:46] local.ALERT: Aufgabe ausgeführt!  
[202x-mm-dd 19:24:47] local.ALERT: Aufgabe ausgeführt!  
[202x-mm-dd 19:24:47] local.ALERT: Aufgabe ausgeführt!  
[202x-mm-dd 19:24:49] local.ALERT: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 19:24:49] local.ALERT: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 19:24:49] local.ALERT: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:37:47] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:37:47] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:37:48] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:37:48] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:40:25] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:40:26] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:40:38] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:40:39] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:40:47] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:40:47] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:40:47] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:40:47] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:40:53] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:40:56] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:41:01] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:41:02] local.INFO: Aufgabe ausgeführt!  
[202x-mm-dd 12:41:02] local.INFO: Aufgabe in der Queue ausgeführt!  
[202x-mm-dd 12:41:02] local.INFO: Aufgabe in der Queue ausgeführt!  
``` 
