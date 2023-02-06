**Übung 12:** Es darf nicht alles hochgeladen werden!

Erstelle einen Datei-Upload, bei dem der Nutzer nur Dateien mit der Endung .png bis 1 MB Dateigröße hochladen kann.

**Lösung:**


1.
in der web.php 2e routes anlegen:

```
use Illuminate\Http\Request;

// uebung_12
// eine Route für das Upload-Formular
Route::get('/upload', function () {
    return view('upload');
});

// und noch eine für die POST-Action:
Route::post('upload', function (Request $request) {

    if ($request->file('image')->isValid()) {

        $request->validate([
            'image' => 'required|max:1024|mimes:png',
        ]);

        //dd($request->image);
        $request->image->store('imagesbilder'); 
        // storage/app/imagesbilder  hier kommts an!!!!
    }
});

```

2. 
ein Formular zum uploaden anlegen:
in resources/views/
upload.blade.php

```
@extends('layouts.app')

@section('content')

<div class="flex items-center">

    <div class="md:w-1/2 md:mx-auto">

        <div class="flex flex-col break-words bg-white border border-2 rounded shadow-md">

            <div class="w-full p-6">

                <form action="upload" method="post" enctype="multipart/form-data">

                    @csrf

                    <div class="sm:col-span-6 mt-6">

                        <input type="file" name="image" accept="image/*" />

                        @error('image')

                        <p class="text-red-500 text-xs italic">{{ $message }}</p>

                        @enderror

                    </div>
                    


                    <div class="mt-8 border-t border-gray-200 pt-5">

                        <div class="flex justify-end">

                            <span class="ml-3 inline-flex rounded-md shadow-sm">

                                <button type="submit"
                                    class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out">

                                    Save

                                </button>

                            </span>

                        </div>

                    </div>

                </form>
              

            </div>

        </div>

    </div>

</div>

@endsection
```

**Test:**

http://advanced-laravel.test/upload

