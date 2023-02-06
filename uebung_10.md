**Übung 10:** Weg von den Gates hin zur Policy

Packe unsere zwei Gates 

zum Aktualisieren des Aktivitätsstatus eines Posts 
und 
zum Ansehen der Posts 

in unsere bereits erstellte Policy.

**Lösung:**

1
Die PostPolicy.php ist vorhanden:
in app\Policies:

```
<?php

namespace App\Policies;

use App\Post;
use App\User;

use Illuminate\Auth\Access\HandlesAuthorization;
use Illuminate\Auth\Access\Response;

class PostPolicy
{
    use HandlesAuthorization;

	  // war schon da!
    public function viewAny(?User $user)
    {
        return true; // neu
    }

    public function togglePost(User $user, Post $post)
    {
        return $post->user->is($user)
        ? Response::allow()
        : Response::deny('You cannot toggle the Post because its not yours');
    }
}
```

2. 
In AuthServiceProvider.php:

Die Gates auskommentieren!


3. 
Entweder in der Blade-View  die Activity Namen von der ..-.. Schreibeweise auf die Methoden-Schreibweise ändern ,
oder:
Die Gates auf die Methoden mappen:

```
Gate::define('post-activity-umschalten', [PostPolicy::class, 'postActivityUmschalten']);

Gate::define('posts-ansehen', [PostPolicy::class, 'postsAnsehen']);
```


