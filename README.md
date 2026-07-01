# Week 6 — Angular Advanced: HTTP Interceptors, Guards & State Management

## Objective
Demonstrate REST API consumption, token-based authentication via HTTP
interceptors, route protection via guards, and lightweight application
state management, using Angular 17 standalone APIs.

## Tech Stack
- Angular 17 (standalone components, functional interceptors/guards)
- Angular Signals for state management
- Reactive Forms
- Backend: Spring Boot REST API (JWT auth) — see "Backend contract" below

## Folder Structure
```
src/app/
├── core/
│   ├── interceptors/
│   │   ├── auth.interceptor.ts     # attaches JWT to outgoing requests
│   │   └── error.interceptor.ts    # global 401/403 handling
│   ├── guards/
│   │   └── auth.guard.ts           # protects routes needing login
│   ├── services/
│   │   ├── auth.service.ts         # login/logout, token + user signals
│   │   ├── product.service.ts      # HttpClient calls to /api/products
│   │   └── app-state.service.ts    # centralised app state (signals)
│   └── models/
│       ├── user.model.ts
│       └── product.model.ts
├── features/
│   ├── login/login.component.ts
│   ├── dashboard/dashboard.component.ts   # guarded route
│   └── products/product-list.component.ts
├── app.routes.ts
├── app.config.ts
└── app.component.ts
```

## How each requirement is implemented

| Requirement | Where | Notes |
|---|---|---|
| Consume REST APIs with HttpClient | `product.service.ts` | Standard CRUD calls; `provideHttpClient()` registered in `app.config.ts` |
| Interceptor for token handling | `auth.interceptor.ts`, `error.interceptor.ts` | Functional interceptors (`HttpInterceptorFn`), registered via `withInterceptors([...])` |
| Route guards for authentication | `auth.guard.ts` | Functional guard (`CanActivateFn`), applied to `/dashboard` in `app.routes.ts` |
| Application state management | `app-state.service.ts`, `auth.service.ts` | Angular Signals: private writable signal + public readonly signal + `computed()` for derived state |

## Setup Instructions
This zip is a complete, runnable Angular workspace — no `ng new` needed.

```bash
cd week6-http-interceptors
npm install
npm start          # same as: ng serve -o
```
This opens `http://localhost:4200`. You'll land on `/login`.

Verified with `ng build` on Angular 17 before packaging — installs and
compiles clean.

## Backend contract expected
Point these at your Spring Boot backend (adjust `apiUrl` in the two
services if your ports/paths differ):

- `POST /api/auth/login` — body `{ username, password }` → returns
  `{ token, user }`
- `GET /api/products` — requires `Authorization: Bearer <token>` header
  (attached automatically by `authInterceptor`)
- Standard REST verbs for `/api/products/{id}` (GET/PUT/DELETE)

Any endpoint returning `401` will trigger an automatic logout + redirect
to `/login` via `errorInterceptor`; a `403` redirects back to
`/dashboard` (customize this for a proper "access denied" page later).

## Key concepts to be ready to explain in review
- **Why functional interceptors/guards over class-based**: Angular 17
  standalone apps favor functions + `inject()` — less boilerplate, no
  `NgModule` needed, and they compose cleanly with `withInterceptors()`.
- **Interceptor execution order**: request-side runs top-to-bottom
  through the array passed to `withInterceptors`; response-side
  (including errors) unwinds bottom-to-top.
- **Why signals over a Subject/BehaviorSubject service**: signals give
  synchronous reads (`state.products()` instead of subscribing), automatic
  change detection integration, and `computed()` for derived values
  without manual `combineLatest` wiring.
- **Guard vs interceptor responsibility split**: the guard decides
  *whether navigation is allowed*; the interceptor decides *what goes
  on the wire* for every request, guarded route or not.

## Possible extensions (not required, good talking points in interviews)
- Refresh-token flow: on 401, attempt silent refresh before logging out.
- Role-based guard (`canActivate` checking `user.role`) for admin-only
  routes.
- Swap `AppStateService` for `@ngrx/signals` `signalStore` once state
  grows beyond a couple of slices.
