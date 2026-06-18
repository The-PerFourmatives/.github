# Frontend Implementation Workflow

This document outlines the standard procedure for implementing UI features using **ASP.NET Core MVC (Razor)**, **Tailwind CSS**, and **jQuery** within the `ASI.Basecode` project.

---

## Phase 1: View Creation & Data Binding
*Goal: Create the page structure and connect it to backend data.*

1.  **Create the View:**
    *   Add a new `.cshtml` file in `ASI.Basecode.WebApp/Views/[ControllerName]/`.
    *   *Example:* If your controller is `BookController`, put the view in `Views/Book/Index.cshtml`.

2.  **Declare the Model:**
    *   At the very top of the file, bind the ServiceModel/ViewModel:
    ```razor
    @model ASI.Basecode.Services.ServiceModels.YourViewModel
    ```

3.  **Define the Layout:**
    *   By default, views use `_Layout.cshtml`. You can set the page title:
    ```razor
    @{
        ViewData["Title"] = "Page Name";
    }
    ```

4.  **Display Data:**
    *   Use Razor syntax to inject model data into HTML:
    ```razor
    <h1 class="text-2xl font-bold">@Model.FullName</h1>
    <p>Email: @Model.Email</p>
    ```

---

## Phase 2: Styling with Tailwind CSS
*Goal: Apply modern, utility-first styling without writing custom CSS.*

1.  **Use Utility Classes:**
    *   Apply Tailwind classes directly to HTML elements. Avoid creating new classes in CSS files.
    *   *Example:* `<div class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">`

2.  **Responsive Design:**
    *   Use prefixes like `md:` or `lg:` for different screen sizes:
    ```html
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4"> ... </div>
    ```

3.  **Compiling Styles:**
    *   If you add new Tailwind classes and they don't seem to apply, ensure the Tailwind compiler is running or rebuild the project.

---

## Phase 3: Global UI Components
*Goal: Maintain consistency across the application.*

1.  **Navigation/Header:**
    *   To add a link to your new page, update `Views/Shared/_Header.cshtml`:
    ```html
    <a asp-controller="Book" asp-action="Index" class="nav-link">Books</a>
    ```

2.  **Common Layout:**
    *   Modify `Views/Shared/_Layout.cshtml` if you need to add global scripts (like Google Fonts) or shared sidebars.

3.  **Localization:**
    *   Use the `ASI.Basecode.Resources` project for UI text:
    ```razor
    @using ASI.Basecode.Resources.Views
    <button>@Button.Save</button>
    ```

---

## Phase 4: Client-Side Logic & Validation
*Goal: Add interactivity and instant feedback.*

1.  **Form Validation:**
    *   For forms, always include the validation scripts at the bottom of your view:
    ```razor
    @section Scripts {
        <partial name="_ValidationScriptsPartial" />
    }
    ```

2.  **Interactive Scripting:**
    *   Add page-specific logic in `wwwroot/js/` or directly in a `@section Scripts`.
    *   **jQuery** is pre-installed. Use it for event handling or AJAX:
    ```javascript
    $(document).ready(function() {
        $('#myButton').on('click', function() {
            // Your logic here
        });
    });
    ```

---

## Checklist for New Pages
- [ ] Controller action returns the correct View.
- [ ] `@model` is declared at the top of the View.
- [ ] Tailwind utility classes are used for styling.
- [ ] Navigation links in `_Header.cshtml` are updated.
- [ ] Form fields use `asp-for` for binding.
- [ ] Validation partial is included if the page has a form.
- [ ] UI strings are pulled from `ASI.Basecode.Resources` (if localized).
