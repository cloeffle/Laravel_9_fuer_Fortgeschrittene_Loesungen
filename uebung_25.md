**Übung 25:** Ein neuer Job

Erstelle einen Job, um eine E-Mail zu versenden,
wenn eine Datei erfolgreich hochgeladen wurde.

**Lösung:**

Vorarbeiten, so fern diese noch nicht da sind:

Upload muss funktionieren Aufgaben 12/13!!!

1.
Einen neuen Job anlegen:

```

art make:job ConfirmUpload

```

Der Job ensteht im Ordner: app\Jobs als ConfirmUpload.php


2.
Eine neue Mail anlegen:

```

art make:mail UploadConfirmation

```

Die mail entsteht im Ordner: app\Mail als UploadConfirmation.php


3.
Die 3 Methoden envelope(), content() und attachments() auskommentieren oder entfernen!

Die build()-Methode für die view einfügen:

```

    public function build()
    {
        return $this->view('mail.upload-confirmation');
    }


```

4. 
Für die Mail wird eine view erstellt,
im Ordner resources\views\mail\ 

upload-confirmation.blade.php erstellen (VON HAND!)


5.
Die View bekommt folgenden Inhalt:

```
<h1>
  Datei erfolgreich hochgeladen
</h1>
```

6
In der Job-Datei aus 1. :

app\Jobs\ConfirmUpload.php:

```
use App\Mail\UploadConfirmation;
use Illuminate\Support\Facades\Mail;

class ConfirmUpload implements ShouldQueue
{
  .......
    public $email;

	public function __construct($email)
	{
		//
		$this->email = $email;
	}
		
	public function handle()
	{
		Mail::to($this->email)->send(new UploadConfirmation());
		//Mail::to(auth()->user()->send( new UploadConfirmation() ));
	}

...
}
```

7.
In der web.php:

die uebung_13 modifizieren:

```
use App\Jobs\ConfirmUpload;
```

In der post-Route zum speichern:

```

dispatch(new ConfirmUpload(auth()->user()->email) );

```

8.

```
php artisan queue:work
```

9.
Upload eines Bildes durchführen!

**Test:**
In mailtrap sollte eine Mail angekommen sein!

