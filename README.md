# 🏺 Heritage Center

> An ASP.NET Core MVC web app for cataloging and managing traditional Palestinian heritage items — pottery, embroidery, olive wood carvings, and more — with separate Admin and Person (member) roles.

---

## 📚 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Default Accounts](#default-accounts)
- [Data Model](#data-model)

---

## 📖 Overview

Heritage Center is a session-based ASP.NET Core MVC application where registered **Persons** own and list **Heritage Items** (e.g. Keffiyeh, Embroidered Fabrics, Olive Wood Carvings, Oil Lamps, Palestinian Pottery), each with a name and price. An **Admin** account (the first seeded person) can manage all items across all persons, while regular members manage and search only their own listings.

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Framework | ASP.NET Core MVC (.NET 6.0) |
| ORM | Entity Framework Core 6.0 (SQL Server) |
| Database | SQL Server LocalDB |
| Auth | Session-based (no ASP.NET Identity) |
| Frontend | Razor Views, Bootstrap 5, jQuery, jQuery Validation |

## ✨ Features

- **Login / Sign Up** — simple name + password + ID login backed by a `Person` table
- **Role split** — session `UID == 1` routes to the Admin area, everyone else routes to the Person area
- **Admin**
  - View all items (`ItemListAdmin`) or all items with owner names (`ItemAndNameList`)
  - Add / Edit / Delete any heritage item
  - Search items by name
  - Change own password
- **Person (member)**
  - View only their own items (`ItemListPerson`)
  - Search their own items by name
  - Change own password
- **Session management** — login state stored server-side via `HttpContext.Session`; logout clears the session

## 🗂️ Project Structure

```
HeritageCenter/
├── Controllers/
│   ├── HomeController.cs      # Login, SignUp, Privacy
│   ├── AdminController.cs     # Admin CRUD + search
│   └── PersonController.cs    # Member item list + search
├── Models/
│   ├── Person.cs
│   ├── HeritageItem.cs
│   └── HeritageContext.cs     # DbContext + seed data
├── Migrations/                # EF Core migrations
├── Views/
│   ├── Home/                  # Login, SignUp, Success/Wrong states
│   ├── Admin/                 # Add, Edit, Delete, item lists, search
│   ├── Person/                # Item list, search, change password
│   └── Shared/                # _Layout, _ValidationScriptsPartial
├── wwwroot/                   # Bootstrap, jQuery, static assets
├── appsettings.json           # Connection string lives here
└── Program.cs
```

## 🚀 Getting Started

### Prerequisites
- [.NET 6.0 SDK](https://dotnet.microsoft.com/download/dotnet/6.0)
- SQL Server LocalDB (ships with Visual Studio) or any SQL Server instance

### Setup

```bash
# 1. Restore dependencies
dotnet restore

# 2. Apply EF Core migrations to create the database
dotnet ef database update

# 3. Run the app
dotnet run
```

The app launches at `http://localhost:5003` by default (see `Properties/launchSettings.json`).

### Connection String

Defined in `appsettings.json`:

```json
"HeritageContextStr": "Server=(localdb)\\mssqllocaldb;Database=Heritage7;Trusted_Connection=True;MultipleActiveResultSets=true"
```

Update this if you're using a different SQL Server instance.

## 🔑 Default Accounts

Seed data is defined in `Models/HeritageContext.cs`. `PersonId = 1` (Ahmad) is treated as the **Admin** account; all others land in the Person area.

| PersonId | Name | Password | Role |
|---|---|---|---|
| 1 | Ahmad | 123 | Admin |
| 2 | Kalid | 345 | Person |
| 3 | Said | 678 | Person |
| 4 | Marah | 444 | Person |
| 5 | Bilal | 467 | Person |

> ⚠️ Passwords are stored in plain text in this project — fine for a coursework/demo app, but should not be used as-is in production.

## 🗃️ Data Model

**Person**
- `PersonId`, `Name`, `Password`, `PhoneNumber`, `Email`, `Address`

**HeritageItem**
- `HeritageItemId`, `ItemName`, `Price`, `PersonId` (FK → Person)

Seeded items include Keffiyeh, Embroidered Fabrics, Olive Wood Carvings, Oil Lamps, and Palestinian Pottery, distributed across the seeded persons.
