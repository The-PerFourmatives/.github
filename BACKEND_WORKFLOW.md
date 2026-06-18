# Backend Implementation Workflow

This document outlines the standard procedure for implementing backend features or modifying existing logic within the `ASI.Basecode` N-Tier architecture.

---

## Phase 1: The Data Layer (`ASI.Basecode.Data`)
*Goal: Manage the database schema and raw data access.*

### Case A: Creating a New Feature
1.  **Model:** Add a new entity in `Models/` (e.g., `Book.cs`).
2.  **DbContext:** Add a `DbSet<T>` for the new model in `AsiBasecodeDbContext.cs`.
3.  **Migration:**
    ```powershell
    dotnet ef migrations add Add[Feature]Table --project ASI.Basecode.Data --startup-project ASI.Basecode.WebApp
    dotnet ef database update --project ASI.Basecode.Data --startup-project ASI.Basecode.WebApp
    ```
4.  **Interface:** Create `I[Feature]Repository.cs` in `Interfaces/`.
5.  **Repository:** Create `[Feature]Repository.cs` in `Repositories/`.
    *   **Inherit** from `BaseRepository`.
    *   **Constructor:** Must pass `IUnitOfWork` to `base(unitOfWork)`.
    *   **Saving:** Use `UnitOfWork.SaveChanges()` inside repository methods.

### Case B: Modifying an Existing Model
1.  **Update Model:** Add/edit properties in the existing class (e.g., `User.cs`).
2.  **Migration:** Run the `migrations add` and `database update` commands listed above.
3.  **Update Repository:** If the change requires new query logic (e.g., searching by a new field), update the Repository and its Interface.
4.  **Update Interface:** Ensure the method (e.g., `GetUserById`) is present in the `Interfaces/I[Feature]Repository.cs`.

---

## Phase 2: The Services Layer (`ASI.Basecode.Services`)
*Goal: Business logic and data transformation (Entity → ViewModel).*

1.  **Service Model (DTO/ViewModel):**
    *   Create/Update a class in `ServiceModels/` (e.g., `BookViewModel.cs`).
    *   **Rule:** This is a "lite" version of the Data Model. Never include sensitive fields like `Password` or `Salt`.
2.  **Interface:** Create/Update `I[Feature]Service.cs` in `Interfaces/`.
3.  **Service Class:** Create/Update `[Feature]Service.cs` in `Services/`.
    *   **DI:** Inject the corresponding **Repository** and `IMapper`.
    *   **Mapping:** Convert Data Entities to ServiceModels before returning them to the WebApp.
        *   *AutoMapper:* `_mapper.Map<Target>(source)`
        *   *Manual:* `new ViewModel { Prop = entity.Prop }`

---

## Phase 3: The WebApp Layer (`ASI.Basecode.WebApp`)
*Goal: Handle HTTP requests and Dependency Injection.*

1.  **Dependency Injection:** 
    *   Register your new Repository and Service in `Startup.DI.cs`:
    ```csharp
    services.AddScoped<I[Feature]Repository, [Feature]Repository>();
    services.AddScoped<I[Feature]Service, [Feature]Service>();
    ```
2.  **Controller:** 
    *   Create/Update a controller in `Controllers/` (e.g., `BookController.cs`).
    *   **DI:** Inject the **Service interface** only (never inject the Repository here).
    *   **Action:** Call the service, receive the ViewModel, and pass it to the `View(model)`.

---

## Key Rules & Definitions

### 1. DTO vs. ViewModel
In this project, these terms are interchangeable. They refer to the classes in the `ServiceModels` folder. Their job is to protect the database structure by only exposing what is necessary to the frontend.

### 2. The "Drilling Holes" Analogy
When you add a new column to a database table, you must "drill a hole" through every layer for that data to reach the UI:
1.  **Data Model** (The storage)
2.  **Repository** (The retrieval)
3.  **ServiceModel/ViewModel** (The container)
4.  **Service** (The mapping/logic)
5.  **View** (The display)

### 3. Unit of Work & BaseRepository
*   **BaseRepository** provides the shared `DbContext`.
*   **UnitOfWork** acts as the supervisor for transactions. 
*   Always use `UnitOfWork.SaveChanges()` within your Repository methods to ensure data is committed to SQL Server.

### 4. Naming Conventions
*   **Interfaces:** Always start with `I` (e.g., `IUserService`).
*   **Repositories:** End with `Repository` (e.g., `UserRepository`).
*   **Services:** End with `Service` (e.g., `UserService`).
*   **ViewModels:** End with `ViewModel` (e.g., `UserViewModel`).
