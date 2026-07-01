# Assignment 1: HTTP Interceptors

## Week 6 – Angular Advanced

## Objective

The objective of this assignment is to demonstrate the implementation of advanced Angular concepts including REST API consumption, HTTP interceptors, route guards, and application state management while maintaining clean coding standards and proper documentation.

---

## Features Implemented

### 1. REST API Integration

* Consumed RESTful APIs using Angular `HttpClient`.
* Implemented CRUD operations for communicating with backend services.
* Handled API responses and errors efficiently.

### 2. HTTP Interceptors

* Created Angular HTTP interceptors for handling authentication tokens.
* Automatically attached JWT/Bearer tokens to outgoing API requests.
* Managed unauthorized responses (`401 Unauthorized`) globally.
* Redirected users to the login page when authentication fails.

### 3. Route Guards

* Implemented route guards to protect authenticated routes.
* Prevented unauthorized users from accessing restricted pages.
* Allowed access only to authenticated users.

### 4. Application State Management

* Managed application state using Angular services and reactive programming concepts.
* Shared user authentication state across components.
* Updated UI dynamically based on authentication status.

---

## Project Structure

```text
src/
│
├── app/
│   ├── components/
│   ├── services/
│   ├── interceptors/
│   ├── guards/
│   ├── models/
│   ├── pages/
│   ├── shared/
│   └── app-routing.module.ts
│
├── assets/
└── environments/
```

---

## Technologies Used

* Angular
* TypeScript
* Angular HttpClient
* RxJS
* Route Guards
* HTTP Interceptors
* REST APIs

---

## Installation and Setup

### Clone the Repository

```bash
git clone <repository-url>
cd <project-folder>
```

### Install Dependencies

```bash
npm install
```

### Start Development Server

```bash
ng serve
```

Navigate to:

```text
http://localhost:4200
```

---

## Interceptor Workflow

1. User logs into the application.
2. Authentication token is stored locally.
3. Interceptor captures every outgoing HTTP request.
4. Token is added to the request header automatically.
5. Backend validates the token and returns the response.
6. If the token is invalid or expired, the interceptor handles the error and redirects the user to the login page.

---

## Route Guard Workflow

1. User attempts to access a protected route.
2. Route guard checks authentication status.
3. If authenticated, access is granted.
4. Otherwise, the user is redirected to the login page.

---

## Learning Outcomes

After completing this assignment, the following concepts were understood and implemented:

* REST API consumption using Angular HttpClient.
* Authentication token handling using HTTP interceptors.
* Securing routes with Angular route guards.
* Managing application state effectively.
* Following clean coding practices and documentation standards.

---

## Author
Shreyash Sable 
