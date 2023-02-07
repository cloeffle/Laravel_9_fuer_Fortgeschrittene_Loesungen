**Übung 18:** Eine Biene in einer Mail. Was hat es damit wohl auf sich?

Erstelle eine Mail-Klasse Image. 

Füge ein eingebettetes Bild sowie Platzhaltertext 
zu einem von der Mail verwendeten View Template hinzu.


**Lösung:**

1.

```

php artisan make:mail Image 

```

app\Mail\Image.php wird erstellt!


2.
im resources\views\mail Ordner
image.blade.php erstellen


3.
in der Image.php Klasse folgende Anpassung machen::

```
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class Image extends Mailable
{
    use Queueable, SerializesModels;

	
    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->markdown('mail.image'); 
    }
}

```


4.
In resources\views\mail Ordner in der:

image.blade.php einp-Tag und ein img-Tag(2 Varianten) eintragen:


```

<p>
    Bienen sind toll. Sie produzieren leckeren Honig!
</p>

<img width="100px" src="{{asset('/img/bee.jpg')}}" />

<img width="100px" src="{{$message->embed(asset('/img/bee.jpg'))}}" />

```

5.
Im public\img Ordner ein Bienen-Bild einfügen
bee.jpg 

**Test:**

NOCH WIRD DIESE MAIL NICHT VERSENDET!
=====================================
