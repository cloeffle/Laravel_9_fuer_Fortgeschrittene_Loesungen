**Übung 31:** Deine erste Custom Exception

Erstelle eine CalcException. 

Wenn die Exception auftritt, soll error, falsche Handhabung von calc in die Logs geschrieben werden. 

Der Nutzer soll die Meldung erhalten, die der CalcException als message mitgegeben wird.

**Lösung:**

1.
Eine eigene Exception-Klasse erstellen:

```
art make:exception CalcException --render --report
```

2.
In app\Exceptions gibt es CalcException.php,
mit den report() und render() Methoden:

```
<?php

namespace App\Exceptions;

use App\Mail\Quote;
use Exception;
use Illuminate\Support\Facades\Mail;

class CalcException extends Exception
{
    /**
     * Report the exception.
     *
     * @return void
     */
    public function report()
    {
        info('error, falsche Handhabung von calc');

        Mail::to('bug@laravel.io')->send(new Quote());
    }

    /**
     * Render the exception as an HTTP response.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function render($request)
    {
        return $this->message ? $this->message : abort(500);
		
		// oder eine view ausgeben
    // vlt, auch eine eigene in views/errors mit z.b: 555.blade.php !
		// return view('home');
    }
}
```


3.
In der web.php die Exception werfen/auslösen:

```
/* 
	uebung_31
*/
use App\Exceptions\CalcException;
Route::get('calc',function(){
	throw new CalcException(); // ohne Nachricht
	
	throw new CalcException('huups da gab es einen fehler!'); // mit Nachricht
	
});
```

**Test:**

http://advanced-laravel.test/calc


Im log nachschauen:
storage\logs\laravel.log


