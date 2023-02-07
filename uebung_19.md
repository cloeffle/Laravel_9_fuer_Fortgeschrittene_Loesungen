**Übung 19:** Endlich die erstellten E-Mails ansehen

Schau deine drei E-Mails der bisherigen Übungen im Browser an und sende sie, 
bis auf die E-Mail mit eingebettetem Bild, an deine Log-Datei.


**Lösung:**

1.
mit dem Browser ansehen:

in der web.php folgende route einfügen:

```
use App\Mail\Quote;
use App\Mail\Image;
use App\Mail\Inspire;

// uebung_19
Route::get('/ansicht_im_browser', function () {

    // die 3 Versionen nacheinander ausprobieren,
    // jeweils die return statements in-/auskommntieren
    
    // 1 
    return new Quote();
	
    // 2
    //return new Image();
		
    // 3
    $quote = 'coffee in the morning saves your life';
    $author = 'wise old man';
    //return new Inspire($quote, $author);
   
});
```

2

an die Logs versenden, 

in der web.php folgende route einfügen:

```
use Illuminate\Support\Facades\Mail;

Route::get('/ansicht_in_logs', function () {

    // 1
    Mail::to('test@test.de')->send(new Quote());
	
    // 2
    Mail::to('test@test.de')->send(new Image());
    
    // 3
    $quote = 'coffee in the morning saves your life';
    $author = 'wise old man';
    Mail::to('test@test.de')->send(new Inspire($quote, $author));

    
});
```

**Tests:**

http://advanced-laravel.test/ansicht_im_browser

http://advanced-laravel.test/ansicht_in_logs





