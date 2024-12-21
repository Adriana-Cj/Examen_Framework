# Examen_Framework

## Arhitectura aplicatie
### Modelele: 

#### Category.php
  
![image](https://github.com/user-attachments/assets/823f718d-c75c-41a9-b4e7-495dcc0163d5)
#### MenuItem.php
![image](https://github.com/user-attachments/assets/843f6227-02ed-4092-ac29-b62f62816240)

### Controalere:
- CategoryController.php
- MenuItemController.php

### Views
- /menu-items/create.blade.php
- /menu-items/index.blade.php

### Schema bazei de date
![image](https://github.com/user-attachments/assets/2f60f479-3b3a-4b4e-ac00-b495aad7745b)

### Tipuri de stocare
- In baza de date se stocheaza informatia despre categoriile meniului si insusi meniul.

### Stack-ul tehnologic utilizat
- Laravel 10.x
- PHP 8.1+
- MySQL
- Bootstrap pentru frontend
- XAMPP ca server local


## Pasii pentru cearea proiectului:
composer create-project laravel/laravel meniu-app
php artisan make:model Category -m
php artisan make:model MenuItem -m
php artisan make:controller CategoryController --resource
php artisan make:controller MenuItemController --resource
php artisan migrate
php artisan serv
php artisan make:seeder CategorySeeder

## Modelele din Controler:
#### Category:
```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Category extends Model
{
    protected $fillable = [
        'nume',
        'descriere'
    ];

    public function menuItems()
    {
        return $this->hasMany(MenuItem::class);
    }
}
```

#### MenuItem:
```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class MenuItem extends Model
{
    protected $fillable = [
        'category_id',
        'nume',
        'descriere',
        'pret',
        'disponibil'
    ];

    public function category()
    {
        return $this->belongsTo(Category::class);
    }
}
```


## Validarea datelor:
#### CategoryRequest:
```
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class CategoryRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [
            'nume' => 'required|string|max:100',
            'descriere' => 'nullable|string|max:500'
        ];
    }
}
```

#### MenuItemRequest:
```
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class MenuItemRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [
            'category_id' => 'required|exists:categories,id',
            'nume' => 'required|string|max:100',
            'descriere' => 'nullable|string|max:500',
            'pret' => 'required|numeric|min:0',
            'disponibil' => 'boolean'
        ];
    }
}
```

## Vizualizari:
#### Layouts/app.blade:
```
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title') - Restaurant</title>
</head>
<body>
    <div>
        <nav>
            <a href="{{ route('menu-items.index') }}">Meniu</a>
            <a href="{{ route('categories.index') }}">Categorii</a>
        </nav>
    </div>

    <div>
        @yield('content')
    </div>
</body>
</html>
```

#### menu-items/create.blade
```
@extends('layouts.app')

@section('title', 'Adaugă Element Nou')

@section('content')
    <h1>Adaugă Element Nou în Meniu</h1>

    @if ($errors->any())
        <div style="color: red;">
            <ul>
                @foreach ($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form method="POST" action="{{ route('menu-items.store') }}">
        @csrf
        
        <div>
            <label>Categorie:</label>
            <select name="category_id">
                @foreach($categories as $category)
                    <option value="{{ $category->id }}">{{ $category->nume }}</option>
                @endforeach
            </select>
        </div>

        <div>
            <label>Nume:</label>
            <input type="text" name="nume" value="{{ old('nume') }}">
        </div>

        <div>
            <label>Descriere:</label>
            <textarea name="descriere">{{ old('descriere') }}</textarea>
        </div>

        <div>
            <label>Preț:</label>
            <input type="number" name="pret" step="0.01" value="{{ old('pret') }}">
        </div>

        <div>
            <label>
                <input type="checkbox" name="disponibil" value="1" checked>
                Disponibil
            </label>
        </div>

        <button type="submit">Salvează</button>
    </form>

    <a href="{{ route('menu-items.index') }}">Înapoi la listă</a>
@endsection
```

#### menu-items/index.blade
```
@extends('layouts.app')

@section('title', 'Meniu Restaurant')

@section('content')
    <h1>Meniu Restaurant</h1>
    
    <a href="{{ route('menu-items.create') }}">Adaugă Element Nou</a>

    <div>
        @foreach($menuItems as $item)
            <div style="border: 1px solid #ccc; margin: 10px; padding: 10px;">
                <h3>{{ $item->nume }}</h3>
                <p>Categorie: {{ $item->category->nume }}</p>
                <p>Preț: {{ $item->pret }} lei</p>
                <p>Descriere: {{ $item->descriere }}</p>
                <p>Status: {{ $item->disponibil ? 'Disponibil' : 'Indisponibil' }}</p>
                
                <a href="{{ route('menu-items.edit', $item->id) }}">Editează</a>
                
                <form method="POST" action="{{ route('menu-items.destroy', $item->id) }}" style="display: inline;">
                    @csrf
                    @method('DELETE')
                    <button type="submit">Șterge</button>
                </form>
            </div>
        @endforeach
    </div>
@endsection
```









