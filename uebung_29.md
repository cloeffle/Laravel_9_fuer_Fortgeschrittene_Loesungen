**Übung 29:** Die erste Bestellung in deiner Imbissbude

Erstelle 

1.
eine Closure Route /ordercompleted 

2.
und sende das Event OrderCompleted bei Aufruf der Route ab.


**Lösung:**

1.

In der web.php eine Closure-Route erstellen :

```
use App\Events\OrderCompleted;

// uebung_29
Route::get('ordercompleted', function (){
	event(new OrderCompleted());
});

```

2.

http://advanced-laravel.test/ordercompleted


3.

da kommt nichts! 


4.

aus dem Tutorial:

Die beiden Listener benötigen in ihren jeweiligen handle()-Methoden z.B. eine dump()-Ausgabe:

```
public function handle(OrderCompleted $event)
{
  dump("Listener xy ausgelöst!");
}
```

**Test:**


http://advanced-laravel.test/ordercompleted

jetzt schlagen die Listener an!
