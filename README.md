# Todo App - README

## Project Overview
This is a simple **To-Do Application** built using **Spring Boot**, **Spring Security**, **Thymeleaf**, and **MySQL** for managing projects and associated tasks (To-Dos). It allows users to create, update, delete, and manage projects and to-do items. It also includes authentication, project exports, and task completion features.

## Features
- **Authentication**: Basic authentication using Spring Security with in-memory user details.
- **CRUD Operations**: Create, Read, Update, and Delete for both projects and to-dos.
- **Form-based Login**: Users can log in via a custom login form.
- **Project and Todo Management**: Add, delete, edit, and mark tasks as complete.
- **Project Export**: Export projects in markdown format.
- **MySQL Database**: Uses MySQL for persistent storage of projects and to-dos.

## Prerequisites
- **Java 17+**
- **MySQL Server**
- **Maven**
- **Spring Boot**

## Technologies Used
- **Spring Boot**: Backend framework for creating RESTful APIs and web applications.
- **Spring Security**: Handles authentication and authorization.
- **Thymeleaf**: Templating engine for building dynamic HTML views.
- **MySQL**: Database for storing project and todo data.
- **Hibernate**: ORM framework used with MySQL.

## Configuration

### 1. Application Properties
The application properties are located in `src/main/resources/application.properties` and include the following configurations:

```properties
spring.application.name=todo-app
server.port=8090

# MySQL Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/todoapp?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=

# Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.thymeleaf.cache=false

# Security Configuration
security.user.name=root
security.user.password=root
```

### 2. MySQL Database Setup
Ensure MySQL is installed and running. You will need to create a database named `todoapp`:

```sql
CREATE DATABASE todoapp;
```

Set your MySQL `username` and `password` in the `application.properties` file.

### 3. Running the Application
To run the application, execute the following Maven command:

```bash
mvn spring-boot:run
```

The application will start on [http://localhost:8090](http://localhost:8090).

### 4. Accessing the Application
Once the application is running:
- Go to [http://localhost:8090/login](http://localhost:8090/login).
- Log in with the default credentials:
  - **Username**: `root`
  - **Password**: `root`

You will be redirected to the projects page upon successful login.

## Security Configuration
The security configuration is handled in the `SecurityConfig` class (`com.todo_app.config.SecurityConfig`), which uses basic form-based login and HTTP Basic authentication for API access.

- **Username/Password**: Configurable in `application.properties`.
- **Login**: Custom login page with redirection to `/projects` upon success.
- **Logout**: Custom logout URL and redirect to `/login?logout`.

```java
http
  .csrf(csrf -> csrf.disable())
  .authorizeHttpRequests(authz -> authz
      .requestMatchers("/projects/**").authenticated() // Protects project-related URLs
      .anyRequest().permitAll() // Allows public access to all other endpoints
  )
  .formLogin(form -> form
      .defaultSuccessUrl("/projects", true)
      .permitAll()
  )
  .logout(logout -> logout
      .logoutUrl("/logout")
      .logoutSuccessUrl("/login?logout")
      .permitAll()
  )
  .httpBasic(withDefaults());
```

## Endpoints

- **`/projects`**: List all projects.
- **`/projects/{id}`**: View project details.
- **`/projects` (POST)**: Create a new project.
- **`/projects/{id}/todos` (POST)**: Add a to-do to a project.
- **`/projects/delete/{id}` (POST)**: Delete a project.
- **`/projects/{projectId}/todos/delete/{todoId}` (POST)**: Delete a specific to-do item.
- **`/projects/{projectId}/todos/{todoId}/complete` (POST)**: Mark a to-do as complete.
- **`/projects/{projectId}/todos/{todoId}/edit` (POST)**: Update a specific to-do item.
- **`/projects/{projectId}/edit` (POST)**: Update the project title.
- **`/projects/{projectId}/export` (POST)**: Export the project in markdown format.

## Project Structure
```
src/
├── main/
│   ├── java/
│   │   └── com/todo_app/
│   │       ├── config/           # Spring Security Config
│   │       ├── controller/       # Controller Layer
│   │       ├── entity/           # JPA Entities (Project, Todo)
│   │       ├── repository/       # Data Repositories
│   │       └── service/          # Service Layer
│   └── resources/
│       ├── templates/            # Thymeleaf templates (HTML)
│       ├── application.properties # Configuration file
```

## Usage
- **Login** with the default credentials.
- **Create a Project** by filling in the project form.
- **Add To-Dos** to a project by entering the task description.
- **Edit/Delete Projects and To-Dos**.
- **Mark To-Dos as Complete**.
- **Export Projects** in Markdown format.


