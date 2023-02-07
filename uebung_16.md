**Übung 16:** Wir brauchen eine reine Textversion der Mail

Erstelle für die Mail Quote eine reine Textversion, die den Text 'reine Text Mail' wiedergibt.

**Lösung:**

Voraussetzung: uebung_15.md

1.

in app\Mail\Quote.php den return Wert auf die view ändern:

```

return $this->view('mail.quote')->text('mail.quote_plain');

```


2. 

noch eine View im views\mail Ordner erstellen und nachfolgenden Text (ohne HTML) einfügen:

quote_plain.blade.php:

```

“Any fool can write code that a computer can understand. Good programmers write code that humans can understand.”

```

**Test:**

NOCH WIRD DIESE MAIL NICHT VERSENDET!
=====================================
