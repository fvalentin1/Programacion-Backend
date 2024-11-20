# Pasos para crear Semillas de Usuarios
Las semillas o seeds son registros que se pueden agregar de forma predeterminada a su proyecto.

## Paso 1: Ejecutar comando para crear semilla
Se debe colocar el nombre del modelo en singular, acompañado de "Seeder". Por ejemplo para salas (rooms), será `RoomSeeder`.

```bash
php artisan make:seeder RoomSeeder
```
Este comando creará en `carpeta_raiz_de_su_proyecto/database/seeders/` el archivo `RoomSeeder.php`.

## Paso 2: Definir las semillas
Se debe abrir el archivo `RoomSeeder.php` y agregar las siguientes líneas.

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use App\Models\Room;

class RoomSeeder extends Seeder
{
    public function run()
    {
        Room::create(['name' => 'Conference Room', 'capacity' => 50, 'location' => 'First Floor']);
        Room::create(['name' => 'Meeting Room', 'capacity' => 20, 'location' => 'Second Floor']);
        Room::create(['name' => 'Training Room', 'capacity' => 30, 'location' => 'Third Floor']);
    }
}
```

## Paso 3: Configurar las semillas
El archivo `DatabaseSeeder.php` ubicado en `carpeta_raiz_de_su_proyecto/database/seeders/` debe incluir el llamado a `RoomSeeder`.

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(UserSeeder::class); // Debe estar anteriormente creado, si no tiene semillas de usuario, debe eliminar esta línea.

        $this->call(RoomSeeder::class); // Línea de referencia a RoomSeeder.php
    }
}
```

## Paso 4: Ejecutar las semillas

Si se desean cargar solo las semillas pendientes, ejecutar:
```bash
php artisan db:seed
```

En cambio, si se desea cargar migraciones como tambien las semillas desde cero, ejecutar:
```bash
php artisan migrate:fresh --seed
```