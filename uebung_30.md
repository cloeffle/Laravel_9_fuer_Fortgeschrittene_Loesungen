**Übung 30:** Wir sind Detektive und observieren das Post Model

Erstelle einen Observer für das Post-Model. 

Jedes mal wenn ein neuer Post erstellt wurde, 

soll eine Benachrichtigung an alle Nutzer der Applikation gesendet werden.


**Lösung:**

0.

Das Post Modell MUSS VORHER vorhanden sein!!!
In Uebung_08 wird das erstellt!!!!!!!!!!!!!!!!!!!!!!

Ansonsten das User Model nehmen!


1.

Den Observer erstellen für post bzw. user - model

```
php artisan make:observer PostObserver -m Post

oder 

php artisan make:observer UserObserver -m User
```

2.

PostObserver.php bzw. UserObserver.php ensteht im App/Observers Ordner:

In diesem Observer wird in allen Methoden-Köpfen (Post $post) bzw. (User $user) benutzt

use App\Models\Post;

bzw. 

use App\Models\User;


3.

Eine Notification erstellen:

```
php artisan make:notification PostCreated

bzw.

php artisan make:notification UserCreated
```

4.

in PostCreated.php:

```
	public $post;
    
	public function __construct($post)
    	{
       	 //
       	 $this->post = $post;
    	}
	
	public function toMail($notifiable)
    	{
        	return (new MailMessage)
                    ->line("A new Post with the Title:\"{$this->post->title}\"  was created.");
    	}
```

bzw.

in UserCreated.php:

```
	public $user;
    
	public function __construct($user)
    	{
        	//
        	$this->user = $usert;
    	}
	
	public function toMail($notifiable)
    	{
        	return (new MailMessage)
                    ->line("A new User with the Name:\"{$this->user->name}\"  was created.");
    	}
```

	
5.

in PostObserver.php:


``` 	
	use App\Models\Post;
 	use App\Models\User;
	use App\Notifications\PostCreated;
	use Illuminate\Support\Facades\Notification;

 
	public function created(Post $post)
    	{
       	 	//
        	logger($post->title); //debug log level im default channel
        	Notification::send(User::all(), new PostCreated($post));
    	}

```

bzw.

``` 	
  	use App\Models\User;
	use App\Notifications\PostCreated;
	use Illuminate\Support\Facades\Notification;

 
	public function created(User $user)
    	{
        	//
        	logger($user->name); //debug log level im default channel
        	Notification::send(User::all(), new UserCreated($user));
    	}

```

5.

in tinker:

```
$user = User::find(1); // vorher prüfen, ob dein user die id=1 hat! ansonsten anpassen! ;-)
$user->post()->create(['title' => 'zweiter Post', 'body' => 'das ist doch toll!']);
```

oder:

```
User::create(['name' => 'Testname', 'email' => 'test@test.de', 'password' => '!Geheim123']);
```



6.

In AppServiceProvider.php muss noch der Observer in der boot()-Methode aktiviert werden!:

```
	use App\Models\Post;

	public function boot()
    	{
        	//
        	Post::observe(\App\Observers\PostObserver::class);
    	}
```

bzw. für das User-Model:
```
	use App\Models\User;

	public function boot()
    	{
        	//
        	User::observe(\App\Observers\UserObserver::class);
    	}
```
	
7. 

und nochmal Schritt 5 mit neuen Daten wiederholen!


**Test:**

Voila!

Im Log und mailtrap nachschauen! ;-)




	

