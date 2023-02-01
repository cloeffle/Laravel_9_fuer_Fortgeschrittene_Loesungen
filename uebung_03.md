**Übung 3:** Middlewares – eine zwiebelige Angelegenheit

Schreibe eine Before Middleware und eine After Middleware,
die für die get Route onion ausgeführt werden sollen. 
 
Die Before Middleware soll »before layer« und 

die After Middleware »after layer« 

mit »echo« wiedergeben. 

Das Closure der Route soll »core« wiedergeben.

**Lösung:**

1.

2 Middleware Controller erstellen für before und after:


```
php artisan make:middleware BeforeLayer

php artisan make:middleware AfterLayer

```

in app/Http/Middleware

werden die beiden Dateien erstellt!

BeforeLayer.php 
AfterLayer.php


2.

in der web.php erstellen wir eine get-Route:


```

// aufgabe_03
Route::get('/onion', function () {
    echo " core ";
})->middleware('before.Layer','after.Layer');

```


3.

in den Middlewares geben wir jeweils ein echo für "after" und "before" aus!!

in BeforeLayer.php :

```
class BeforeLayer
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle(Request $request, Closure $next)
    {
        echo "before layer ";
        return $next($request);
    }
}

```
in AfterLayer.php:

```
class AfterLayer
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle(Request $request, Closure $next)
    {
        $response = $next($request);
        echo ' after layer';
        return $response;
    }
}
```


4.
in der Kernel.php muessen die beiden auch registriert werden :

```

 'easteregg' => \App\Http\Middleware\EasterEgg::class, // aus der vorherigen Übung bereits vorhanden!
 'before.Layer' => \App\Http\Middleware\BeforeLayer::class,
 'after.Layer' => \App\Http\Middleware\AfterLayer::class,

```

 
 **Test:**
 
 http://advanced-laravel.test/onion
 
 
