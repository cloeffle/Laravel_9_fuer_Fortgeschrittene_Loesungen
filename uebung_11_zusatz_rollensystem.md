**Ein Rollensystem wird implementiert**

1.
Das Model Role mit der notwendigen Migration erstellen:

```

php artisan make:model Role -m

```

2.
Die Migration ändern und migrieren:

```
Schema::create('roles', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('label')->nullable();
    $table->timestamps();
});
Schema::create('abilities', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('label')->nullable();
    $table->timestamps();
});
Schema::create('ability_role', function (Blueprint $table) {
    $table->primary(['role_id', 'ability_id']);
    $table->foreignId('role_id');
    $table->foreignId('ability_id');
    $table->timestamps();
    $table->foreign('role_id')->references('id')->on('roles')->onDelete('cascade');
    $table->foreign('ability_id')->references('id')->on('abilities')->onDelete('cascade');
});
Schema::create('role_user', function (Blueprint $table) {
    $table->primary(['user_id', 'role_id']);
    $table->foreignId('user_id');
    $table->foreignId('role_id');
    $table->timestamps();
    $table->foreign('role_id')->references('id')->on('roles')->onDelete('cascade');
    $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
});
```

```

php artisan migrate 

```

3.
Ein Model für die Berechtigungen anlegen :

```

php artisan make:model Ability

```

4.
Many-To-Many Beziehungen in den Models eintragen:

```
//user model
public function roles()
{
    return $this->belongsToMany(Role::class)->withTimestamps();
}


//role model
protected $guarded = []; // Mass-Assignment

public function abilities()
{
    return $this->belongsToMany(Ability::class)->withTimestamps();
}


//ability model
protected $guarded = []; // Mass-Assignment

public function roles()
{
    return $this->belongsToMany(Role::class)->withTimestamps();
}
```

5.
Zuweisungsmethoden in den Models erstellen:

```
//role model
public function assignAbility($ability)
{
    return $this->abilities()->syncWithoutDetaching($ability);
}

//user model
public function abilities()
{
    return $this->roles->map->abilities->flatten()->pluck('name')->unique();
}

public function assignRole($role)
{
    return $this->roles()->syncWithoutDetaching($role);
}
```

6.
Jetzt mit tinker ausprobieren:

```
php artisan tinker 
```

```
//in tinker

//meine user id ist 1
$user = Auth::loginUsingId(1);

$role = App\Role::firstOrCreate([
  'name' => 'administrator',
]);

$ability = App\Ability::firstOrCreate([
    'name' => 'always-toggle-post',
]);

$role->assignAbility($ability);

$user->assignRole($role);

//alle rollen erhalten
$user->roles;

//alle berechtigungen erhalten
$user->abilities();

?>
//ausgabe:
=> Illuminate\Support\Collection {#1003
     all: [
       "always-toggle-post",
     ],
   }
```

7.
In ASP AuthServiceProvider.php ein Gate hinzufügen:

```
public function boot()
    {
        $this->registerPolicies();
        
        ....
        
        Gate::before(function ($user, $ability) {
            if ($user->abilities()->contains($ability)) {
                return true;
            }
        });
        
        ....
```

8.
In der View muss der Link nun wie folgt geschützt werden:

```

@canany(['togglePost', 'always-toggle-post'], $post)
<a href="post/{{$post->id}}/toggle">toggle activity</a>
@endcanany

```

9.
Die Route zum Umschalten muss ebenfalss angepasst werden:

```
Route::get('post/{post}/toggle', function (App\Post $post) {
    if (Gate::none(['togglePost', 'always-toggle-post'], $post)) {
        abort(403);
    }
    $post->toggleActivity();

    return redirect('/');
});


```
