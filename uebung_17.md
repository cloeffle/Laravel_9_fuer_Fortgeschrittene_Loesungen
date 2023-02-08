**Übung 17:** Inspirierende Mail die dynamische Mails versendet

Erstelle eine neue Komponente für Zitate 

und binde diese in eine neue erstellte Mail mit dem Namen »Inspire« ein.

Im Zitat soll der Text angezeigt werden, der über die $slot-Variable weitergegeben wird. 

Außerdem soll neben dem Zitat der Autor übergeben und in der Komponente angezeigt werden.

**Lösung:**

1.
noch eine Mail erzeugen, diesmal die Klasse Inspire.php:

```

php artisan make:mail Inspire --markdown="mail.inspire"

```

app\Mail\Inspire.php
und
resources\mail Ordner 

und darin inspire.balde.php ( MIT VORGEFÜLLTEM INHALT!)
werden automatisch angelegt!  

AUCH der ORDNER "mail", wenn er noch nicht da war !!!


2.
in Inspire.php folgenden Code einsetzen:

```
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class Inspire extends Mailable
{
    use Queueable, SerializesModels;

	// public Eigenschaften sind in der blade-View sichtbar, ohne Übergabe!
    public $quote;
    public $author;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($quote, $author)
    {
        //
        $this->quote = $quote;
        $this->author = $author;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->markdown('mail.inspire'); // $quote und $author sind in der blade vorhanden!
    }
}

```


3.
in resources\views\mail:

inspire.blade.php:

Vorsicht! keinen Auto-formatter in vs-code benutzen, markdown kann eine nervige Sache sein ;-)


```
@component('mail::message')
# Quote

Hi, ist die Mail angekommen?

@component('mail::quote',['author' => $author])
{{$quote}} # Slot Variable!
@endcomponent

Viele Grüße<br>

@endcomponent
```

4.
in resources\views\vendor\mail\html:

quote.blade.php:

```
<p>
    <i>
        {{$slot}}
    </i>
    <strong>{{$author}}</strong>
</p>
```

5.
in resources\views\vendor\text\html:

quote.blade.php:

```
{{$slot}} by {{$author}}
```

**Test:**

NOCH WIRD DIESE MAIL NICHT VERSENDET!
=====================================
