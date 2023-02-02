**Übung 6:** Schreibe selbst Code und implementiere das Login System

Baue in deine Laravel-Applikation das Login-System ein,
falls du dies noch nicht gemacht hast. 

Ein Nutzer soll sich anmelden, registrieren und abmelden können. 

ACHTUNG:
E-Mail-Verifizierung und Zurücksetzen des Passworts sind NICHT Teil der Übung. 

Ich habe dazu kein Lösungsvideo gedreht. Vielmehr sind die einzelnen Videos dieser Lektion die Lösung. :) 

Nutze diese Übung als Selbsttest, ob du das Thema verstanden hast.

**Lösung: gemeinsam im Unterricht**



1. 
installieren des socialite-system:

```

composer require laravel/socialite

``` 


2.
im /config Ordner die services.php Datei enthaelt die provider-Eintraege:

Die Schnippsel kommen aus der socialite-website!!!!

```

'github' => [
    'client_id' => env('GITHUB_KEY'),
    'client_secret' => env('GITHUB_SECRET'),
    'redirect' => env('GITHUB_REDIRECT_URI')
],

```

3. 
github oauth registrieren

https://github.com/settings/applications/new

```
Application Name: advanced-laravel
Homepage URL: http://advanced-laravel.test
Authorization Callback URL: http://advanced-laravel.test/auth/github/callback

Generate Client Secret:
10XXXXcd57add7287785ec27beccf7bb6ccab520
```

https://github.com/settings/developers
	
4.
in der .env Datei alles eintragen!!!:

```
GITHUB_KEY="abc04f6061xxxxx1b141"
GITHUB_SECRET="108xxxxd57add7287785ec27beccf7bb6ccab520"
GITHUB_REDIRECT_URI="http://advanced-laravel.test/auth/github/callback"
```

5.
im LoginController.php:

app\Http\Controllers\Auth\LoginController.php

```

use App\Http\Controllers\Controller;
use App\Providers\RouteServiceProvider;
use Illuminate\Foundation\Auth\AuthenticatesUsers;
use App\Models\SocialAuth;
use App\Models\User;
use Exception;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Facade;

use Laravel\Socialite\Facades\Socialite;
    public function redirectToProvider()
{
    return Socialite::driver('github')->redirect();
}
public function handleProviderCallback()
{
    $oauthUser = Socialite::driver('github')->user();
}   

```

6.
in der web.php 2 routes

```
Route::get('auth/github', 'Auth\LoginController@redirectToProvider');
Route::get('auth/github/callback', 'Auth\LoginController@handleProviderCallback');
```

7.
in  /Http/Controllers/Auth\LoginController.php

2 Methoden einsetzen

```
public function redirectToProvider()
{
    return Socialite::driver('github')->redirect();
}

public function handleProviderCallback()
{
    $oauthUser = Socialite::driver('github')->user(); 
    dump($oauthUser); // erster Test, ob Github authentifiziert hat!
    
    
}
```

	
8.
Neue Migration und Model für 1:1 Beziehung zum User-Modell erzeugen : SocialAuth

```

php artisan make:model SocialAuth -m 

```

9. 
Die Migration anpassen

```
Schema::create('social_auths', function (Blueprint $table) {
    $table->id();
    $table->bigInteger('user_id');
    $table->string('provider_name');
    $table->string('provider_id')->unique();
    $table->timestamps();
});
```` 
10.
migrieren

```

php artisan migrate

```

11.
in User.php App\Models:

```
public function oauth()
{
    return $this->hasOne('App\Models\SocialAuth');
}
```


12.
In SocialAuth.php  Model:

```
protected $fillable = ['user_id', 'provider_name', 'provider_id'];

public function user()
{
    return $this->belongsTo('App\Models\User');
}

```


13.
Im LoginController.php:
 
 ```
  public function handleProviderCallback()
    {
        $oauthUser     = Socialite::driver('github')->user();

        $existingOAuth = SocialAuth::where('provider_name', 'github')
            ->where('provider_id', $oauthUser->getId())
            ->first();

        if ($existingOAuth) {

            $oauthUser=  $existingOAuth->user;

        } else {

            $user = User::whereEmail($oauthUser->getEmail())->first();

            if (!$user) {
                $user = User::create([
                    'email' => $oauthUser->getEmail(),
                    'name' => $oauthUser->getName(),
                ]);
            }

            $user->oauth()->create([
                'provider_id' => $oauthUser->getId(),
                'provider_name' => 'github',
            ]);

            $oauthUser=  $user;
        }

        
        Auth::login($oauthUser, true);

        return redirect($this->redirectTo);
    }

```


14.

```
->nullable();
```

in user migration für password einfügen und fresh migrieren

```
php artisan migrate:fresh
```

**Test:**

User Login mit Github:

http://advanced-laravel.test/login



