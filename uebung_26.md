**Übung 26:** Im Job passieren auch Fehler

Erstelle einen Job der eine Exception wiedergibt 

und dispatche diesen in der Closure Route /. 

Starte deinen Worker.


**Lösung:**

1.
```
art make:job Exception
```

2. 
in app\Jobs liegt Exception.php:

in der handle()-Methode:

```
public function handle()
{
	throw new Exception;
}
```

3. 
In der web.php:

```
// uebung_26
use App\Jobs\Exception;

Route::get('/uebung_26', function(){
	$quote = 'coffee in the morning save your life';
	$author = 'wise old man';
	
	//Mail::to('test@test.de')->send(new Inspire($quote,$author);

	// uebung_26
	dispatch(new Exception()); // use Direktive für diese Klasse muss noch angelegt werden !!! im Video nicht zu sehen !!!!
	
	
	return view('welcome',['posts' => Post::all()]);
});
```

4.
```
php artisan queue:work
```

5.

http://advanced-laravel.test/uebung_26 


6.

Den Worker stoppen und neustarten!

**Test:**

Faild! 

muss im Terminal auftauchen !


