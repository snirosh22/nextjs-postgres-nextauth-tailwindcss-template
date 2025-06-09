# Architecture Overview

This project is a template for an admin dashboard built with the Next.js App Router. It integrates Postgres for data storage, Auth.js (NextAuth) for authentication, Tailwind CSS for styling and Shadcn UI components.

## Directory Structure

- `app/` – Next.js App Router pages and layouts
  - `layout.tsx` – global layout
  - `login/` – GitHub login page
  - `(dashboard)/` – grouped routes for the dashboard
    - `layout.tsx` – dashboard layout with navigation
    - `page.tsx` – main Products page fetching data from the database
    - `customers/` – placeholder customers page
    - Components such as `search.tsx`, `product.tsx`, `products-table.tsx`, `user.tsx`
  - `api/` – server routes
    - `auth/[...nextauth]/route.ts` – NextAuth handlers
    - `seed/route.ts` – optional database seeding
- `lib/` – server utilities
  - `auth.ts` – NextAuth configuration (GitHub provider)
  - `db.ts` – Drizzle ORM schema and helpers for Postgres
  - `utils.ts` – helper for Tailwind class merging
- `components/` – shared React UI components (Shadcn UI based)

## Flow

1. Users access pages through Next.js App Router.
2. Middleware (`middleware.ts`) applies `auth` to protect routes.
3. Authentication uses NextAuth with the GitHub provider.
4. Dashboard pages fetch product data via Drizzle ORM (`lib/db.ts`) from Postgres.
5. UI is styled with Tailwind and Shadcn components.

```mermaid
flowchart TD
    subgraph Next.js App
        A[Root Layout] --> B((Login Page))
        A --> C((Dashboard Group))
        C --> D[Dashboard Layout]
        D --> E[ProductsPage]
        D --> F[CustomersPage]
    end
    subgraph Authentication
        B -->|signIn| G[NextAuth GitHub]
        C --> H(Middleware auth)
        H --> D
        UserComp[User component] -->|auth()| G
    end
    subgraph Data
        E -->|getProducts()| I[lib/db.ts]
        I -->|Drizzle ORM| J[(Postgres)]
    end
```
