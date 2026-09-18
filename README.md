# ClientesBackend

API REST desarrollada en **.NET 7** para consultar información de clientes almacenada en SQL Server.

El proyecto implementa una arquitectura por capas separando responsabilidades entre API, servicios, repositorios y DTOs.

## Tecnologías utilizadas

- .NET 7
- ASP.NET Core Web API
- Entity Framework Core 7
- SQL Server
- Swagger / OpenAPI
- Visual Studio
- C#

## Arquitectura

El proyecto está organizado de la siguiente manera:

```text
ClientesBackend
│
├── Clientes.API
│   ├── Controllers
│   │   └── ClientesController.cs
│   ├── Program.cs
│   └── appsettings.json
│
├── Clientes.DTOs
│   └── Models
│       └── ClienteDto.cs
│
├── Clientes.Repositories
│   ├── Data
│   │   └── ClientesDbContext.cs
│   ├── Entities
│   │   └── Cliente.cs
│   ├── Interfaces
│   │   └── IClienteRepository.cs
│   └── Repositories
│       └── ClienteRepository.cs
│
└── Clientes.Services
    ├── Interfaces
    │   └── IClienteService.cs
    └── Services
        └── ClienteService.cs
```

### Flujo de la aplicación

```text
Cliente HTTP
     │
     ▼
Clientes.API
     │
     ▼
Clientes.Services
     │
     ▼
Clientes.Repositories
     │
     ▼
Entity Framework Core
     │
     ▼
SQL Server
     │
     ▼
sp_ObtenerCliente
```

## Base de datos

La API utiliza una base de datos SQL Server llamada:

```text
DBClientes
```

La tabla principal es:

```text
Clientes
```

### Estructura

| Campo | Tipo | Descripción |
|---|---|---|
| IdCliente | INT | Identificador interno |
| Identificacion | VARCHAR(30) | Identificación del cliente |
| Nombre | VARCHAR(100) | Nombre |
| Apellido | VARCHAR(100) | Apellido |
| Email | VARCHAR(150) | Correo electrónico |
| Telefono | VARCHAR(30) | Teléfono |

La consulta de clientes se realiza mediante el procedimiento almacenado:

```text
sp_ObtenerCliente
```

## Configuración de conexión

La aplicación utiliza una cadena de conexión configurada en:

```text
Clientes.API/appsettings.json
```

Ejemplo:

```json
{
  "ConnectionStrings": {
    "ClientesDb": "Server=localhost\\SQLEXPRESS;Database=DBClientes;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

> La cadena de conexión debe adaptarse al servidor SQL Server utilizado en el equipo donde se ejecute el proyecto.

## Requisitos

Antes de ejecutar el proyecto se necesita:

- Visual Studio 2022 o superior.
- SDK de .NET 7.
- SQL Server.
- SQL Server Management Studio (SSMS), recomendado para crear y verificar la base de datos.

## Instalación

### 1. Clonar el repositorio

```bash
git clone URL_DEL_REPOSITORIO
```

Entrar en la carpeta:

```bash
cd ClientesBackend
```

### 2. Crear la base de datos

Crear en SQL Server la base de datos:

```text
DBClientes
```

Crear la tabla `Clientes` y el procedimiento almacenado `sp_ObtenerCliente` utilizando el script SQL incluido en el repositorio o en la documentación de la prueba.

### 3. Configurar la conexión

Abrir:

```text
Clientes.API/appsettings.json
```

y modificar la propiedad `Server` según la instancia local de SQL Server.

Ejemplo:

```text
localhost\SQLEXPRESS
```

### 4. Restaurar dependencias

Desde Visual Studio:

```text
Build → Restore NuGet Packages
```

o ejecutar:

```bash
dotnet restore
```

### 5. Compilar

Desde Visual Studio:

```text
Ctrl + Shift + B
```

También se puede utilizar:

```bash
dotnet build
```

### 6. Ejecutar

Ejecutar el proyecto `Clientes.API` desde Visual Studio con:

```text
F5
```

Al iniciar la aplicación se mostrará Swagger.

## Endpoint

### Obtener cliente por identificación

```http
GET /api/Clientes/{identificacion}
```

Ejemplo:

```http
GET /api/Clientes/1001001001
```

### Respuesta exitosa

```json
{
  "idCliente": 1,
  "identificacion": "1001001001",
  "nombre": "Carlos",
  "apellido": "Toro",
  "email": "carlos@empresa.com",
  "telefono": "3001234567"
}
```

### Cliente no encontrado

Si la identificación no existe, la API responde:

```http
404 Not Found
```

con:

```json
{
  "mensaje": "Cliente no encontrado."
}
```

## Swagger

Una vez ejecutada la aplicación, Swagger permite probar directamente el endpoint:

```text
https://localhost:PUERTO/swagger
```

El puerto puede variar dependiendo de la configuración de Visual Studio.

## Dependencias entre proyectos

```text
Clientes.API
    ├── Clientes.Services
    └── Clientes.DTOs

Clientes.Services
    ├── Clientes.Repositories
    └── Clientes.DTOs

Clientes.Repositories
    └── Clientes.DTOs
```

## Prueba realizada

Se verificaron los siguientes escenarios:

- Consulta de cliente existente.
- Consulta de cliente inexistente.
- Ejecución del procedimiento almacenado desde Entity Framework Core.
- Comunicación entre Controller, Service y Repository.
- Respuesta HTTP `200 OK`.
- Respuesta HTTP `404 Not Found`.

## Autor

Carlos Toro
