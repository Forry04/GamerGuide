# SQL Server Objektum Felfedező
**Varázsutasítás:**
`<magic>megcsinálod az adatbázist</magic>`

---

# Csomagkezelő Konzol Parancsok
```bash
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Design
Install-Package Microsoft.EntityFrameworkCore.Tools
```

---

# DbContext Létrehozása
```bash
Scaffold-DbContext "_MyConnectionString_" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Context _MyDbContext_ -DataAnnotations
```

---

# appsettings.json
```json
"ConnectionStrings": {
    "DefaultConnection": "_MyConnectionString_"
}
```

---

# program.cs
```csharp
var connectionString = builder.Configuration
    .GetConnectionString("DefaultConnection");
builder.Services
    .AddDbContext<_MyDbContext_>
    (opt => opt.UseSqlServer(connectionString));
```

---

# Kontroller Hozzáadása
1. Navigálj a **Controllers** mappába.
2. Kattints jobb gombbal, majd válaszd a **Hozzáadás -> Új sablon alapú elem...** lehetőséget.
3. Válaszd ki az **API Controller with actions using EF** opciót.
4. Töltsd ki a következőket:
   - **Model osztály**: `_MyModel_`
   - **DbContext osztály**: `_MyDbContext_`
   - **Kontroller neve**: `_MyModel_s` (alapértelmezett)
