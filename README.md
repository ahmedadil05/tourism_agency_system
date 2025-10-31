
# Project Title

A brief description of what this project does and who it's for

This is a documentation of the "Tourism Agency System" project based on the provided source code and configuration files.

### 1. Project Overview and Setup

The "Tourism Agency System" is a desktop management application developed in Java. It utilizes the Swing toolkit for its Graphical User Interface (GUI) and connects to a PostgreSQL database.

* **Technology Stack:** Java with Swing GUI.
* **Database:** PostgreSQL.
* **JDBC Driver:** `postgresql-42.6.0`.
* **Database Connection Details** (from `core/Db.java`):
    * URL: `jdbc:postgresql://localhost:5432/tourismagencysystem`
    * Username: `postgres`
    * Password: `postgres`

### 2. Core Components

| Class/File | Package | Description |
| :--- | :--- | :--- |
| `Db.java` | `core` | Implements the **Singleton pattern** to ensure a single, consistent connection to the PostgreSQL database for the application. |
| `Helper.java` | `core` | Provides utility functions, including: setting the GUI theme to "Nimbus", displaying alert and confirmation messages (`showMsg`, `confirm`), validating empty text fields, and centering windows on the screen. |
| `ComboItem.java` | `core` | A simple data structure to hold key-value pairs, typically used for combo box items. |
| `Layout.java` | `view` | The base abstract class for all application windows (extending `JFrame`). It handles basic window initialization (sizing, centering, title) and contains helper methods for managing `JTable` data models. |

### 3. Data Model and Logic

#### Entity
* **`entity/User.java`**: Represents a user record in the system.
    * **Fields**: `id`, `name`, `password`, `role`.

#### Data Access Object (DAO)
* **`dao/UserDao.java`**: Handles direct SQL interactions with the `public.user` table in the database.
    * **Key Methods**: `findAll()`, `save()`, `update()`, `delete()`, `findByLogin(username, password)`, and `getById(id)`.

#### Business Logic Layer
* **`business/UserManager.java`**: Implements the business logic for user management.
    * It wraps the `UserDao` operations, providing methods for finding, saving, updating, and deleting users.
    * It also contains a method (`getForTable`) to format user data into an `Object[]` list suitable for display in a `JTable`.

### 4. User Interfaces (Views)

The application uses different views for different user roles, instantiated based on a successful login.

#### A. LoginView (`view/LoginView.java`)
* **Purpose**: The main entry point for the application.
* **Fields**: Text field for "User Name" and a password field for "Password".
* **Navigation**: Upon successful login, users are routed based on their role:
    * If the role is "Admin", "ADMIN", or "admin", they are directed to `UserManagementView`.
    * Otherwise, they are directed to `EmployeeView`.

#### B. UserManagementView (`view/UserManagementView.java`)
* **Purpose**: The main administrative panel for managing user accounts.
* **Features**:
    * Displays a welcome message to the logged-in administrator.
    * Contains a `JTable` listing all users (ID, Name, Password, Role).
    * **Action Buttons**: **Add**, **Update**, and **Delete** users.
    * The Add/Update/Delete functions can also be accessed via a **right-click context menu** on the user table.
    * Includes a **Logout** button.
    * A filter combo box (`cmbx_user_filter`) is present to filter by `ADMIN` or `EMPLOYEE`.

#### C. UserView (`view/UserView.java`)
* **Purpose**: A dedicated pop-up window for adding a new user or editing an existing user's details.
* **Fields**: Text fields for **Username** and **Password**, and a `JComboBox` for selecting the **Role**.
* **Roles**: The available roles are hardcoded as **ADMIN** and **EMPLOYEE**.
* **Save Logic**: The "Save" button triggers either a `save()` (for a new user) or `update()` (for an existing user) operation via the `UserManager`.

#### D. EmployeeView (`view/EmployeeView.java`)
* **Purpose**: The main panel for standard employee users.
* **Features**:
    * Displays an "Employee Panel" title and a welcome message.
    * Includes a `JTabbedPane` labeled "Untitled" (with binding `tabbedPane_hotel`), which is intended for core business features like hotel and/or room management.
    * A `JTable` component is present within this tabbed pane.

### 5. Application Entry Point (`App.java`)

The `main` method in `App.java` is responsible for initializing the application.

* It first sets the application's look and feel.
* **Note**: The current version of `App.java` has the `LoginView` commented out and directly launches the `EmployeeView` using hardcoded login credentials (`"ahmed"`, `"2025"`) for testing purposes. To use the login functionality, the commented lines would need to be re-enabled.
