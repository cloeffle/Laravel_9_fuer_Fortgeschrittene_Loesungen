**Übung 2:** Deine erste eigene Middleware – das Osterei

Wir benötigen nachfolgend eine erste selbst erstellte Middleware.

Diese hat keine wirklich sinnvolle Aufgabe. 

Wir wollen im Path /welcome ein kleines Easteregg verstecken. 

Erstelle daher eine Middleware mit dem Namen EasterEgg.

**Lösung:**

1.

Die Middleware wird mit einem artisan command erstellt:

```

php artisan make:middleware EasterEgg 

```

**Test:**

In app/Http/Middleware ensteht die Datei 

EasterEgg.php



**Im Skript geht das Beispiel weiter:**

**Lösung:**

2.

Die Middleware muss auch in kernel.php registriert werden!!
in app/Http/kernel.php :

```

'easteregg' => \App\Http\Middleware\EasterEgg::class,

```

3.

Einen Request in der web.php als route ausloesen, an die die Middleware gebunden wird:

z.B.:

```

Route::get('/egg', function () {
    //return view('welcome');
})->middleware('easteregg');;

```

4.

Die Middleware Klasse anpassen:

```
use Illuminate\Support\Carbon;

class EasterEgg
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
	// uebung_02 und beispiel aus skript
	if ($request->easteregg) {
            $easter = Carbon::createFromDate(null, 4, 12);
            if ($easter->isToday()) {
                return redirect()->away('https://images.unsplash.com/photo-1522184462610-d9493b82cdf2?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=564&amp;q=80');
            } else {
                abort(403, 'Its not easter yet');
            }
        }
        return $next($request);
    }

}
```

**Test:**

http://advanced-laravel.test/egg?easteregg=true


