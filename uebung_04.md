**Übung 4:** IP-Adressen-Logging

Kleine Übung zum Logging: 

Nutze für die Route '/' den Log Channel daily. 

Dieser soll alle Logs speichern, ohne diese nach einer gewissen Zeit zu überschreiben. 

Es soll in der Log-Datei die IP-Adresse jedes Nutzers geloggt werden, der die Route aufruft.

**Lösung:**

1.

logging.php bearbeiten

default z.B. aendern oder ..


```

'daily' => [
            'driver' => 'daily',
            'path' => storage_path('logs/laravel.log'),
            'level' => env('LOG_LEVEL', 'debug'),
            'days' => 14,
        ],

 'browserlog' => [
            'driver' => 'daily',
            'path' => storage_path('logs/laravel.log'),
            'level' => env('LOG_LEVEL', 'debug'),
            'days' => 1,
        ],

``` 		

2. in web.php:


```
use Illuminate\Support\Facades\Log;

Route::get('/', function () {
    
    // mit dem Helper logger()
    logger('Hallo1'); //debug log level im default channel
    logger()->alert('Hilfe2'); //alert log level im default channel
    logger()->channel('browserlog')->alert('Hilfe3'); //alert log level im browserlog channel
    
    // mit der Log-Facade
    Log::channel('browserlog')->info('Hilfe 4 IP:'.request()->ip());

    // in die slack-Platform slack.com	
    Log::channel('slack')->alert('hallo');

   return view('welcome');
});

```

**Test:**

http://advanced-laravel.test


Die Logs landen im storage/logs  Ordner und bei Slack!


