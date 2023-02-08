**Übung 27:** Ein Fehler bedeutet noch lange nicht das Aus.

Entferne die Exception in der Job-Klasse und führe alle fehlgeschlagenen Jobs erneut aus.

**Test:**

1.
In der Job-Klasse der letzten Übung den Fehler entfernen!

in app\Jobs\Exception.php:

```
use Illuminate\Support\Facades\Log;

public function handle(){
	//
	//throw new Exception;
	//throw new GlobalException;
	
	logger('Exception entfernt!');
}
```

2.
Die Log-Datei öffnen, leeren, speichern:
laravel.log

3.
Die Faild-Queue ansehen:
```
php artisan  queue:failed
```

```
+--------------------------------------+------------+---------+--------------------+---------------------+
| ID                                   | Connection | Queue   | Class              | Failed At           |
+--------------------------------------+------------+---------+--------------------+---------------------+
| 6ffb0570-c2aa-4762-8dcf-1fdbd61077bd | redis      | default | App\Jobs\Exception | 202x-xx-xx 20:13:35 |
| b0e80e7a-a2f1-41c0-9b28-d8bb06ad3167 | redis      | default | App\Jobs\Exception | 202x-xx-xx 20:12:44 |
| 0d66eb43-d1db-4809-b117-086d6bb13ee2 | redis      | default | App\Jobs\Exception | 202x-xx-xx 20:12:44 |
| 17afc3f9-247b-4787-bdad-1f69b4971d83 | redis      | default | App\Jobs\Exception | 202x-xx-xx 20:12:44 |
| eeafd771-087f-4ff4-9042-faf817ebe12c | redis      | default | App\Jobs\Exception | 202x-xx-xx 20:12:44 |
+--------------------------------------+------------+---------+--------------------+---------------------+
```

4.
Die Queue wieder neustarten:

```
php artisan  queue:retry all
```

5.
und wieder in der Log-Datei nachsehen:

```
The failed job [6ffb0570-c2aa-4762-8dcf-1fdbd61077bd] has been pushed back onto the queue!
The failed job [b0e80e7a-a2f1-41c0-9b28-d8bb06ad3167] has been pushed back onto the queue!
The failed job [0d66eb43-d1db-4809-b117-086d6bb13ee2] has been pushed back onto the queue!
The failed job [17afc3f9-247b-4787-bdad-1f69b4971d83] has been pushed back onto the queue!
The failed job [eeafd771-087f-4ff4-9042-faf817ebe12c] has been pushed back onto the queue!

Exception entfernt!

```
