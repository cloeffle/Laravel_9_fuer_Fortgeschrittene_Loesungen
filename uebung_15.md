**Übung 15:** Ein Zitat via Email versenden

Erstelle eine Mail-Klasse Quote. 

Für diese sollst du ein Blade Template erstellen. Schreibe in das E-Mail-Template ein Zitat, das versendet werden soll.

**Lösung:**

1.
Eine Mail-Klasse "Quote" erzeugen:

```

php artisan make:mail Quote

```

in app\Mails gibt es nun Quote.php

2.

* Im views-Ordner einen neuen Unterordner mail erstellen
* In mail-Ordner eine quote.blade.php View erstellen



3.
GAAANZ WICHTIG !

entferne die 3 Methoden envelope(), content() und attachments()

4.

in Quote.php die Methode build() einfügen:


```
public function build()
{
  return $this->view('mail.quote');
}


```

5. 
in der eben erstellten View(quote.blade.php) folgendes einfügen:


```

<p>
    “Any fool can write code that a computer can understand. Good programmers write code that humans can understand.”
</p>

```

**Test:**

NOCH WIRD DIESE MAIL NICHT VERSENDET!
=====================================
