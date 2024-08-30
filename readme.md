
## ToDo List Application

# [ToDo List Application - (https://www.youtube.com/watch?v=yuxoESDTCGs) ](https://www.youtube.com/watch?v=yuxoESDTCGs)

### Descripción

La aplicación ToDo List es una aplicación web para la gestión de tareas. Permite a los usuarios autenticarse, agregar, editar, eliminar y buscar tareas. Está desarrollada utilizando React en el frontend y ASP.NET Core con Entity Framework Core en el backend.

### Arquitectura del Proyecto

#### Frontend

- **Tecnologías:** React, Axios, React Router
- **Carpetas Principales:**
  - `src/`: Contiene todos los componentes de React, servicios y archivos relacionados.
  - `src/components/`: Componentes de React reutilizables.
  - `src/services/`: Servicios para la comunicación con el backend.

##### Archivos Importantes

- **`src/App.js`:** Configura las rutas principales de la aplicación.
- **`src/components/TaskList.js`:** Muestra la lista de tareas, permite editar, eliminar y buscar tareas.
- **`src/components/TaskForm.js`:** Formulario para agregar y editar tareas.
- **`src/components/Login.js`:** Componente para el inicio de sesión del usuario.
- **`src/components/ProtectedRoute.js`:** Protege las rutas para asegurar que solo los usuarios autenticados puedan acceder a ciertas páginas.

#### Backend

- **Tecnologías:** ASP.NET Core, Entity Framework Core, PostgreSQL
- **Archivos Principales:**
  - **`ToDoListAPI.csproj`:** Archivo de proyecto que define las dependencias y configuraciones del proyecto backend.
  - **`Controllers/TasksController.cs`:** Controlador para manejar las operaciones CRUD (Crear, Leer, Actualizar, Eliminar) para las tareas.
  - **`Models/TaskItem.cs`:** Modelo de datos para las tareas.
  - **`Program.cs`:** Configuración del servidor y servicios.

##### Archivos Importantes

- **`ToDoListAPI.csproj`:** Contiene las referencias a paquetes necesarios para la aplicación.
- **`TasksController.cs`:** Implementa los métodos para manejar las tareas:
  - `GET /api/tasks`: Obtiene todas las tareas.
  - `POST /api/tasks`: Crea una nueva tarea.
  - `PUT /api/tasks/{id}`: Actualiza una tarea existente.
  - `DELETE /api/tasks/{id}`: Elimina una tarea.

#### Docker

- **Dockerfile:** Define cómo construir la imagen Docker para el backend de la aplicación.

```dockerfile
# Use the official .NET image from the Docker Hub
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["ToDoListAPI/ToDoListAPI.csproj", "ToDoListAPI/"]
RUN dotnet restore "ToDoListAPI/ToDoListAPI.csproj"
COPY . .
WORKDIR "/src/ToDoListAPI"
RUN dotnet build "ToDoListAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ToDoListAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ToDoListAPI.dll"]

## Instrucciones de Uso

### 1. Instalar Dependencias
- **Frontend:**
  ```bash
  cd todo-list-frontend
  npm install

cd ToDoListAPI
dotnet restore

cd ToDoListAPI
dotnet run

cd todo-list-frontend
npm start


docker build -t todolistapi .
docker run -p 5007:80 todolistapi

