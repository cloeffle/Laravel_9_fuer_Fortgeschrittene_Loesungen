**Übung 5:** Erste Schritte mit Laravels Login System

Hier ist wieder eine kleine Aufgabe, mit der ich dich testen möchte: 
Implementiere in deiner Laravel-Applikation, falls noch nicht geschehen, das Login-System. 

Wenn du dir bei einigen Punkten unsicher bist, kannst du natürlich in das Walkthrough schauen. 

Deine Aufgabe ist es, 
das Login-System zu nutzen. 

Auf dem Homescreen, also der URI /home, 
soll der Name des Nutzers angezeigt werden, wenn dieser angemeldet ist. 

Außerdem soll eine URI /secret, die den Text »gesicherter Bereich« wiedergibt, 
nur für angemeldete Nutzer aufrufbar sein.

**Lösung:**

1.

login system muss implementiert sein, siehe Tutorial "Schritt für Schritt1" und Video 8:43


2.

user registrieren mit email

3.

die home route gibt es ja bereits aus er installation!

siehe web.php

```
Auth::routes();

Route::get('/home', [App\Http\Controllers\HomeController::class, 'index'])->name('home');
```



4. 
im Template home.blade.php:

auth()->user()->name hinzufügen!!

```
{{ __('Hi '.auth()->user()->name.'! You are logged in! ') }}
```

5. 

secret route einrichten in web.php:

```
Route::get('/secret',function(){
	return "gesicherter Bereich!";
})->middleware('auth');
```

6.
testen mit angemeldetem und abgemeldetem user!

**Test:**

http://advanced-laravel.test/secret

http://advanced-laravel.test/home




