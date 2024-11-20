# Pasos para crear Semillas de Usuarios
Las semillas o seeds son registros que se pueden agregar de forma predeterminada a su proyecto.

## Paso 1: Ejecutar comando para crear semilla

```bash
php artisan make:seeder UserSeeder
```
Este comando creará en `carpeta_raiz_de_su_proyecto/database/seeders/` el archivo `UserSeeder.php`.

## Paso 2: Definir las semillas
Se debe abrir el archivo `UserSeeder.php` y agregar las siguientes líneas.

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Hash;
use App\Models\User;

class UserSeeder extends Seeder
{
    public function run()
    {
        User::create([
            'name' => 'Admin',
            'email' => 'admin@test.cl',
            'password' => Hash::make('123'),
        ]);

        User::create([
            'name' => 'User',
            'email' => 'user@test.cl',
            'password' => Hash::make('123'),
        ]);
    }
}
```

## Paso 3: Configurar las semillas
El archivo `DatabaseSeeder.php` ubicado en `carpeta_raiz_de_su_proyecto/database/seeders/` debe incluir el llamado a `UserSeeder`.

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(UserSeeder::class);
    }
}
```

## Paso 4: Ejecutar las semillas

Si se desean cargar solo las semillas, ejecutar:
```bash
php artisan db:seed
```

En cambio, si se desea cargar migraciones como tambien las semillas desde cero, ejecutar:
```bash
php artisan migrate:fresh --seed
```