**Übung 33:** Caching der Anfrage an Openfoodfacts

Jetzt wird es Zeit, dass du Caching einsetzt. 

Ich finde, dass sich die Anfragen des HTTP Clients an die Openfoodfacts API dafür sehr gut anbieten. 

Implementiere das Caching so, dass bei mehrfacher Eingabe des Barcodes nur beim ersten Mal eine Anfrage an die API gesendet wird.


**Lösung:**

0.

```
Route::post('product',..) muss schon vorhanden sein siehe Aufgaben vorher!!!
```

1.
In der web.php die Post-Route anpassen:

```
Route::post('product', function(Request $request){

	$request->validate([
		'barcode' => 'required',
	]);

	// diese eine Zeile ist uebung_33!!!!!!!!!!!
	$response = cache()->rememberForever("product.{$request->barcode}", function () {
        return Http::beforeSending( function () {
            info('Anfrage bezüglich des Barcodes'.request()->barcode.'gesendet.');
        })->get('https://de.openfoodfacts.org/api/v0/product/'.request()->barcode)->json();
    });
	
	
	
	return view('product-search', compact('response'));
	
});
```

**Test:** 

Testen mit dem Formular unter:

http://advanced-laravel.test/product

4000417021007 als barcode

In log-Datei nachschauen was passiert:
storage\logs\laravel.log-Datei


zur not chache leeren:
`` 
php artisan cache:clear
```

Mehrmals die seite reloaden,

im log gibt es keine neuen anfragen mehr an die api!


Anderen barcode ausprobieren und im log schauen!!


