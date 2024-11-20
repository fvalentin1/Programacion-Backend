# Código para crear un CRUD o Mantenedor en Laravel

## Paso 1: Crear modelo, controlador y migración
Para crear un CRUD es necesario ejecutar el comando `php artisan make:model Room -mcr`, este comando creará migración, modelo y controlador de Salones o Rooms. El nombre debe ser en Singular, idealmente en inglés y con la primera letra en mayúscula.

```bash
php artisan make:model Room -mcr
```

La migración se ubica en `carpeta_raiz_de_su_proyecto\database\migrations\xxxx_xx_xx_create_rooms_table.php`
El modelo se ubica en `carpeta_raiz_de_su_proyecto\app\Models\Room.php`
El controlador se ubica en `carpeta_raiz_de_su_proyecto\app\Http\Controllers\RoomController.php`

## Paso 2: Configurar la migración
Edita el archivo generado en `database/migrations` para definir la estructura de la tabla `rooms`:

```php
public function up()
{
    Schema::create('rooms', function (Blueprint $table) {
        $table->id();
        $table->string('name');       // Nombre del salón
        $table->integer('capacity');  // Capacidad del salón
        $table->string('location');   // Ubicación del salón
        $table->timestamps();
    });
}
```

## Paso 3: Configurar el modelo Room
Edita el modelo `Room` para especificar los atributos que son asignables masivamente:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Room extends Model
{
    use HasFactory;

    protected $fillable = ['name', 'capacity', 'location'];
}
```

## Paso 4: Definir las rutas
Edita el archivo `routes/web.php` para incluir las rutas protegidas para `rooms`:

```php
use App\Http\Controllers\RoomController; // NO OLVIDAR IMPORTAR EL CONTROLADOR EN LAS RUTAS

Route::group(['middleware' => 'auth'], function () { // MIDDLEWARE PARA EL CONTROL DE SESIONES ACTIVAS
    Route::resource('rooms', RoomController::class);
});
```

## Paso 5: Configurar el controlador
Edita el controlador `RoomController` para implementar los métodos del CRUD:
```php
namespace App\Http\Controllers;

use App\Models\Room;
use Illuminate\Http\Request;

class RoomController extends Controller
{
    public function index()
    {
        $rooms = Room::all();
        return view('rooms.index', compact('rooms'));
    }

    public function create()
    {
        return view('rooms.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'name' => 'required|string|max:255',
            'capacity' => 'required|integer|min:1',
            'location' => 'required|string|max:255',
        ]);

        Room::create($request->all());

        return redirect()->route('rooms.index')->with('success', 'Room created successfully.');
    }

    public function show(Room $room)
    {
        return view('rooms.show', compact('room'));
    }

    public function edit(Room $room)
    {
        return view('rooms.edit', compact('room'));
    }

    public function update(Request $request, Room $room)
    {
        $request->validate([
            'name' => 'required|string|max:255',
            'capacity' => 'required|integer|min:1',
            'location' => 'required|string|max:255',
        ]);

        $room->update($request->all());

        return redirect()->route('rooms.index')->with('success', 'Room updated successfully.');
    }

    public function destroy(Room $room)
    {
        $room->delete();

        return redirect()->route('rooms.index')->with('success', 'Room deleted successfully.');
    }
}
```

## Paso 6: Crear las vistas
Las vistas se ubican en `carpeta_raiz_de_su_proyecto\resources\views\`.

Dentro de `\views` se debe crear una carpeta con el nombre `rooms`. Luego dentro de la carpeta `rooms` creada recientemente, se deben crear las vistas para las 4 funciones principales de un CRUD (Create, Read, Update, Delete).

### 6.1 - Index
La vista de `index` es la vista principal de nuestro CRUD, acá se mostrará un listado de los objetos actualmente creados, además de mostrar sus campos y sus acciones.
Para crear la vista de `index` es necesario crear un fichero o archivo dentro de la carpeta `..\views\rooms` con el nombre `index.blade.php`. La extensión `.blade.php` identifica a las vistas en Laravel.

```html
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Create Room</h1>

    @if ($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach ($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form action="{{ route('rooms.store') }}" method="POST">
        @csrf

        <div class="form-group mb-3">
            <label for="name">Room Name</label>
            <input type="text" name="name" id="name" class="form-control" value="{{ old('name') }}" placeholder="Enter room name" required>
        </div>

        <div class="form-group mb-3">
            <label for="capacity">Capacity</label>
            <input type="number" name="capacity" id="capacity" class="form-control" value="{{ old('capacity') }}" placeholder="Enter room capacity" min="1" required>
        </div>

        <div class="form-group mb-3">
            <label for="location">Location</label>
            <input type="text" name="location" id="location" class="form-control" value="{{ old('location') }}" placeholder="Enter room location" required>
        </div>

        <button type="submit" class="btn btn-primary">Save Room</button>
        <a href="{{ route('rooms.index') }}" class="btn btn-secondary">Cancel</a>
    </form>
</div>
@endsection
```

### 6.2 - Create
La vista de `create` es la vista que permite crear un nuevo registro en nuestro CRUD. Para esto se debe completar el formulario con los respectivos campos.
Para crear la vista de `create` es necesario crear un fichero o archivo dentro de la carpeta `..\views\rooms` con el nombre `create.blade.php`.

```html
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Create Room</h1>

    @if ($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach ($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form action="{{ route('rooms.store') }}" method="POST">
        @csrf

        <div class="form-group mb-3">
            <label for="name">Room Name</label>
            <input type="text" name="name" id="name" class="form-control" value="{{ old('name') }}" placeholder="Enter room name" required>
        </div>

        <div class="form-group mb-3">
            <label for="capacity">Capacity</label>
            <input type="number" name="capacity" id="capacity" class="form-control" value="{{ old('capacity') }}" placeholder="Enter room capacity" min="1" required>
        </div>

        <div class="form-group mb-3">
            <label for="location">Location</label>
            <input type="text" name="location" id="location" class="form-control" value="{{ old('location') }}" placeholder="Enter room location" required>
        </div>

        <button type="submit" class="btn btn-primary">Save Room</button>
        <a href="{{ route('rooms.index') }}" class="btn btn-secondary">Cancel</a>
    </form>
</div>
@endsection
```


### 6.3 - Edit
La vista de `edit` es la vista que permite editar un registro ya existente en nuestro CRUD. Para esto se debe completar el formulario con los respectivos campos, los cuales tendrán el valor actual del campo como parte del formulario.
Para crear la vista de `edit` es necesario crear un fichero o archivo dentro de la carpeta `..\views\rooms` con el nombre `edit.blade.php`.

```html
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Edit Room</h1>

    @if ($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach ($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form action="{{ route('rooms.update', $room->id) }}" method="POST">
        @csrf
        @method('PUT')

        <div class="form-group mb-3">
            <label for="name">Room Name</label>
            <input type="text" name="name" id="name" class="form-control" value="{{ old('name', $room->name) }}" placeholder="Enter room name" required>
        </div>

        <div class="form-group mb-3">
            <label for="capacity">Capacity</label>
            <input type="number" name="capacity" id="capacity" class="form-control" value="{{ old('capacity', $room->capacity) }}" placeholder="Enter room capacity" min="1" required>
        </div>

        <div class="form-group mb-3">
            <label for="location">Location</label>
            <input type="text" name="location" id="location" class="form-control" value="{{ old('location', $room->location) }}" placeholder="Enter room location" required>
        </div>

        <button type="submit" class="btn btn-primary">Update Room</button>
        <a href="{{ route('rooms.index') }}" class="btn btn-secondary">Cancel</a>
    </form>
</div>
@endsection
```

### 6.4 - Show
La vista de `show` es la vista que permite ver un registro ya existente en nuestro CRUD.
Para crear la vista de `show` es necesario crear un fichero o archivo dentro de la carpeta `..\views\rooms` con el nombre `show.blade.php`.

```html
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Room Details</h1>

    <div class="card">
        <div class="card-header">
            <h3>{{ $room->name }}</h3>
        </div>
        <div class="card-body">
            <p><strong>Capacity:</strong> {{ $room->capacity }}</p>
            <p><strong>Location:</strong> {{ $room->location }}</p>
        </div>
        <div class="card-footer">
            <a href="{{ route('rooms.index') }}" class="btn btn-secondary">Back to Rooms</a>
            <a href="{{ route('rooms.edit', $room->id) }}" class="btn btn-warning">Edit Room</a>
            <form action="{{ route('rooms.destroy', $room->id) }}" method="POST" style="display:inline;">
                @csrf
                @method('DELETE')
                <button type="submit" class="btn btn-danger" onclick="return confirm('Are you sure you want to delete this room?')">Delete Room</button>
            </form>
        </div>
    </div>
</div>
@endsection
```

## Paso 7: Cargar las migraciones a la base de datos
Recordar que se debe crear la base de datos en `PhpMyAdmin` con el mismo nombre que tiene en el archivo `.env`.

```.env
DB_DATABASE=nombre_de_su_base_de_datos
```
Luego se debe cargar la base de datos en `PhpMyAdmin` para que funcione correctamente con su proyecto.

Si se desean cargar solo las migraciones pendientes, ejecutar:
```bash
php artisan migrate
```

Si se desea cargar la base de datos junto a las migraciones como tambien las semillas desde cero, ejecutar:
```bash
php artisan migrate:fresh --seed
```
