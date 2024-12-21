# Examen_Framework

## Arhitectura aplicatie
### Modelele: 
- Category.php
![image](https://github.com/user-attachments/assets/823f718d-c75c-41a9-b4e7-495dcc0163d5)
- MenuItem.php
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

## Modelele din Controler:
Category:
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

MenuItem:
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
CategoryRequest:
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

MenuItemRequest:
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
```

#### Menu-Items/
```
```

####
```
```





