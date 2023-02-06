**Übung 13:** Jeder Nutzer erhält seinen eigenen Dateiordner

Beschäftige dich nun ein bisschen mit der Dokumentation der Dateistruktur von Laravel. 

Aufgabe ist es, dass jeder Nutzer Dateien hochladen kann, 
die in einem Ordner mit dem Namen seiner User-ID gespeichert werden. 

In seinem eigenen Ordner soll der Nutzer eigene Ordner erstellen können.


**Lösung:**

1.

3 routes in der web.php anlegen:

```
// uebung_13
use Illuminate\Support\Facades\Storage;

Route::get('upload_uebung_13/', function () {
    $path = 'public/'.auth()->user()->id;
    Storage::makeDirectory($path);
    $files = Storage::allFiles($path);
    $directories = Storage::allDirectories($path);

    return view('upload_uebung_13', compact('files', 'directories'));
});


Route::post('directory', function (Request $request) {
    $path = 'public/'.auth()->user()->id;
    Storage::makeDirectory($path.'/'.$request->directory);

    return redirect('upload_uebung_13/');
});


Route::post('upload_uebung_13', function (Request $request) {
    $path = 'public/'.auth()->user()->id;
    if ($request->file('image')->isValid()) {
        $request->validate([
            'image' => 'required|max:1024|mimes:png',
        ]);

        Storage::putFile($path, $request->image);
        
        return redirect('/upload_uebung_13');
    }
});

```

2. eine view anegen:
uebung_13.blade.php

```
@extends('layouts.app')

@section('content')

<div class="flex items-center">

    <div class="md:w-1/2 md:mx-auto">

        <div class="flex flex-col break-words bg-white border border-2 rounded shadow-md">

            <div class="w-full p-6">

                <form action="upload_uebung_13" method="post" enctype="multipart/form-data">

                    @csrf

                    <div class="sm:col-span-6 mt-6">

                        <input type="file" name="image" accept="image/*" />

                        @error('image')

                        <p class="text-red-500 text-xs italic">{{ $message }}</p>

                        @enderror

                    </div>
                    <div
                        class="mt-6 sm:mt-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5">

                        <label for="disk" class="block text-sm font-medium leading-5 text-gray-700 sm:mt-px sm:pt-2">

                            Dateien auf der Festplatte

                        </label>

                        

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
                <form method="POST" action="directory">
                    @csrf

                    <input type="text" name="directory" required />
                    <button type="submit">Submit</button>
                </form>

                <div>
                    <ul>
                        @foreach ($files as $file)
                        <li>{{$file}}</li>
                        @endforeach
                        @foreach ($directories as $directory)
                        <li>{{$directory}}</li>
                        @endforeach
                    </ul>
                </div>

            </div>

        </div>

    </div>

</div>

@endsection

```

**Test:***

http://advanced-laravel.test/upload_uebung_13

