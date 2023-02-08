**Übung 22:** Benachrichtige deine Datenbank

Führe das Setup für den Channel database durch. 

Versende anschließend die Notification Welcome an die Datenbank.


**Lösung:**

1.

Setup für die Notifications-Tabelle ausführen:

```
php artisan notifications:table

php artisan migrate

```


2. 

Die notification gibt es bereits!

```

php artisan make:notification Welcome

```

3.


in \app\Notifications:

Welcome.php:

```
namespace App\Notifications;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Notifications\Messages\MailMessage;
use Illuminate\Notifications\Notification;
use NotificationChannels\Twitter\TwitterChannel;
use NotificationChannels\Twitter\TwitterStatusUpdate;

class Welcome extends Notification
{
    use Queueable;

    /**
     * Create a new notification instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
        $this->user = auth()->user();
    }

    /**
     * Get the notification's delivery channels.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function via($notifiable)
    {
        return ['database'];
    }

    /**
     * Get the mail representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return \Illuminate\Notifications\Messages\MailMessage
     */
    public function toMail($notifiable)
    {
        return (new MailMessage)
                    ->line('Setzt mal dein Passwort zurück.');
    }

    /**
     * Get the array representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function toArray($notifiable)
    {
        return [
            'message' => 'Thank you for using our application.',
        ];
    }

}
```

4.
In der web.php eine Route erstellen:

```
// uebung_22
use Illuminate\Support\Facades\Notification;
use App\Notifications\Welcome;

Route::get('notification', function () {
    auth()->loginUsingId(10);
    Notification::send(auth()->user(), new Welcome());

	 dd( auth()->user()->notifications );
	 
    return 'notification send';
});

```

**Test:**

http://advanced-laravel.test/notification

