# Visual Studio 2022 Projekt Beállítások
- **Projekt típus**: ASP.NET Core Web API
- **Framework**: .NET 8.0
- **Beállítások**:
  - [x] Place solutioon and project in the same directory
  - [x] Configurate for https
  - [x] Enable Open API Support (swagger)
  - [x] Use controllers (for MVC...)

---

# SQL Server Object Explorer
1. Nyisd meg a **View -> SQL Server Object Explorer** menüt a Visual Studio-ban.
2. Csatlakozz az **MSSQLLocalDB** példányhoz. Az alkalmazás az **MSSQLLocalDB** példányt használja, ezért győződj meg róla, hogy az adatbázis script MSSQL-kompatibilis.
3. Hozz létre egy új lekérdezést (**New Query**).
4. Például:  Futtasd az alábbi parancsokat az adatbázis létrehozásához és használatához:

   ```sql
   -- Adatbázis létrehozása
   CREATE DATABASE GamerGuideDB;

   -- Adatbázis használata
   USE GamerGuideDB;

   -- Tábla létrehozása
   CREATE TABLE Players (
       PlayerID INT PRIMARY KEY IDENTITY(1,1),
       Name NVARCHAR(100) NOT NULL,
       Score INT NOT NULL,
       JoinDate DATETIME DEFAULT GETDATE()
   );
   ```

---

# NuGet Package Manager Console
### Hogyan érheted el a NuGet Package Manager Console-t?
1. Nyisd meg a Visual Studio-t.
2. A menüsorban válaszd ki a **Tools** menüt.
3. Kattints a **NuGet Package Manager -> Package Manager Console** lehetőségre.
4. A konzol megjelenik az alsó panelen.

### Szükséges parancsok
Futtasd az alábbi parancsokat a NuGet Package Manager Console-ban a szükséges csomagok telepítéséhez:

```bash
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Design
Install-Package Microsoft.EntityFrameworkCore.Tools
```

---

# Scaffold-DbContext
Generáld a DbContext osztályt és a modelleket az alábbi paranccsal:

```bash
Scaffold-DbContext "_MyConnectionString_" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Context _MyDbContext_ -DataAnnotations
```

---

# appsettings.json
A kapcsolati sztringet az `appsettings.json` fájlban kell megadni. Példa:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "_MyConnectionString_"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

---

# program.cs
### 1. Kapcsolati sztring beolvasása
```csharp
var connectionString = builder.Configuration
    .GetConnectionString("DefaultConnection");
```

### 2. DbContext regisztrálása
```csharp
builder.Services
    .AddDbContext<_MyDbContext_>(opt => opt.UseSqlServer(connectionString));
```

### 3. CORS engedélyezése
```csharp
builder.Services.AddCors(
    options => options.AddDefaultPolicy(
        builder => builder
            .AllowAnyOrigin()
            .AllowAnyHeader()
            .AllowAnyMethod()
    ));
```

### 4. Middleware beállítása
A CORS middleware-t **a `program.cs` fájl middleware szakaszában kell aktiválni**, **mielőtt** a `app.UseHttpsRedirection()` hívás történik.

```csharp
app.UseCors(); // CORS middleware aktiválása
app.UseHttpsRedirection();
```

---

# API Controller létrehozása
### Hogyan hozhatsz létre API Controllert az Entity Framework használatával?
1. Nyisd meg a **Controllers** mappát a Solution Explorer-ben.
2. Kattints jobb egérgombbal a mappára, majd válaszd az **Add -> New Scaffolded Item...** lehetőséget.
3. A megjelenő ablakban válaszd ki az **API Controller with actions, using Entity Framework** opciót, majd kattints a **Add** gombra.
4. Töltsd ki az alábbi mezőket:
   - **Model class**: `_MyModel_`
   - **Data context class**: `_MyDbContext_`
   - **Controller name**: `_MyModel_sController` (alapértelmezetten megadott, nem szükséges át nevezni)
5. Kattints a **Add** gombra, és a Visual Studio automatikusan generálja a szükséges kódot.