# SecondCircuit — Electronics Marketplace
## Project Specification & Development Plan

**Type:** Portfolio Project  
**Stack:** React + Node.js/Express + MongoDB  
**Author:** tmgpesang01@gmail.com

---

## Overview

SecondCircuit is a second-hand electronics accessories marketplace where users can both buy and sell items. It serves as a portfolio demonstration of a full-stack MERN-style application with real-world features: authentication, marketplace listings, a shopping cart, and an admin panel.

---

## User Roles

| Role    | Description                                              |
|---------|----------------------------------------------------------|
| Guest   | Browse and view product listings                         |
| Buyer   | Registered user — can add to cart, checkout              |
| Seller  | Registered user — can list, edit, and delete own items   |
| Admin   | Full access — manage all users, products, and orders     |

> Note: Buyer and Seller are the same account type. Any registered user can both buy and sell.

---

## Tech Stack

### Frontend
- **React 18** — component-based UI
- **React Router v6** — client-side routing
- **Tailwind CSS** — utility-first styling
- **Axios** — HTTP client for API calls
- **Context API** — auth state and cart state management

### Backend
- **Node.js + Express** — REST API server
- **MongoDB + Mongoose** — document database
- **JSON Web Tokens (JWT)** — stateless auth
- **bcryptjs** — password hashing
- **Multer** — image upload handling
- **dotenv** — environment config

### Dev Tooling
- **Vite** — frontend build tool
- **Nodemon** — backend dev server
- **ESLint + Prettier** — code quality

---

## Data Models

### User
```
_id, name, email, password (hashed), role (user | admin),
avatar, createdAt
```

### Product
```
_id, title, description, price, category, condition (new | like-new | good | fair),
images[], seller (ref: User), status (active | sold | removed),
location, createdAt, updatedAt
```

### Cart (stored client-side in localStorage + synced on login)
```
items: [{ product (ref), quantity, priceAtAdd }]
```

### Order
```
_id, buyer (ref: User), items[], totalAmount,
shippingAddress, status (pending | confirmed | shipped | delivered),
createdAt
```

---

## Pages & Routes

### Public
| Route              | Page                  |
|--------------------|-----------------------|
| `/`                | Home / Featured       |
| `/products`        | Product Listings      |
| `/products/:id`    | Product Detail        |
| `/login`           | Login                 |
| `/register`        | Register              |

### Authenticated
| Route              | Page                  |
|--------------------|-----------------------|
| `/cart`            | Shopping Cart         |
| `/checkout`        | Checkout (mock)       |
| `/orders`          | My Orders             |
| `/sell`            | Create Listing        |
| `/my-listings`     | My Listings           |
| `/my-listings/:id/edit` | Edit Listing    |
| `/profile`         | Profile Settings      |

### Admin
| Route              | Page                  |
|--------------------|-----------------------|
| `/admin`           | Dashboard Overview    |
| `/admin/products`  | Manage All Products   |
| `/admin/users`     | Manage Users          |
| `/admin/orders`    | Manage Orders         |

---

## API Endpoints

### Auth
```
POST   /api/auth/register
POST   /api/auth/login
GET    /api/auth/me
```

### Products
```
GET    /api/products              — list (filter, search, paginate)
GET    /api/products/:id          — single product
POST   /api/products              — create listing (auth)
PUT    /api/products/:id          — update listing (owner/admin)
DELETE /api/products/:id          — delete listing (owner/admin)
```

### Orders
```
POST   /api/orders                — place order (auth)
GET    /api/orders/mine           — buyer's orders (auth)
GET    /api/orders/:id            — single order (auth)
```

### Admin
```
GET    /api/admin/users           — list users
PUT    /api/admin/users/:id       — update user role/status
GET    /api/admin/orders          — list all orders
PUT    /api/admin/orders/:id      — update order status
DELETE /api/admin/products/:id    — remove any product
```

### Uploads
```
POST   /api/upload                — upload product image(s) (auth)
```

---

## Feature Breakdown

### 1. Auth System
- Register with name, email, password
- Login returns a JWT stored in localStorage
- Protected routes redirect to `/login`
- Persistent session via token validation on app load

### 2. Product Listings
- Grid view with card components
- Filter by: category, condition, price range
- Search by title keyword
- Sort by: newest, price low→high, price high→low
- Pagination (12 items per page)

### 3. Product Detail Page
- Image gallery (multiple photos)
- Seller info with listing date
- Condition badge
- "Add to Cart" button
- "Contact Seller" placeholder (no real messaging)

### 4. Shopping Cart
- Persisted in localStorage
- Merged with server on login
- Quantity adjustment + remove item
- Running total

### 5. Mock Checkout
- Step 1: Review cart
- Step 2: Shipping address form
- Step 3: Payment form UI (no real processing — card fields are UI only)
- Step 4: Order confirmation page with order ID

### 6. Sell / Create Listing
- Form: title, description, category, condition, price, images (up to 4), location
- Image upload preview before submit
- Edit and delete own listings

### 7. Admin Dashboard
- Stats: total users, products, orders, revenue
- Product management: view all, remove flagged
- User management: view, promote to admin
- Order management: view all, update status

---

## Project Structure

```
e-commerce/
├── client/                  # React frontend (Vite)
│   ├── src/
│   │   ├── components/      # Reusable UI components
│   │   ├── pages/           # Route-level page components
│   │   ├── context/         # AuthContext, CartContext
│   │   ├── hooks/           # Custom hooks
│   │   ├── services/        # Axios API service functions
│   │   └── utils/
│   └── index.html
│
├── server/                  # Express backend
│   ├── controllers/
│   ├── middleware/          # auth, error handling
│   ├── models/              # Mongoose schemas
│   ├── routes/
│   ├── uploads/             # Local image storage
│   └── server.js
│
├── SPEC.md                  # This file
└── README.md
```

---

## Development Phases

### Phase 1 — Project Setup & Auth
- [ ] Initialize Vite React app + Express server
- [ ] Configure MongoDB connection
- [ ] Build User model + auth routes (register/login)
- [ ] JWT middleware
- [ ] Login and Register pages (frontend)
- [ ] AuthContext with protected routes

### Phase 2 — Product Listings
- [ ] Product model + CRUD routes
- [ ] Product listing page with grid layout
- [ ] Product detail page
- [ ] Search and filter UI
- [ ] Pagination

### Phase 3 — Selling (Create Listings)
- [ ] Image upload (Multer)
- [ ] Create listing form
- [ ] Edit / delete own listings
- [ ] My Listings page

### Phase 4 — Cart & Checkout
- [ ] CartContext (localStorage + sync)
- [ ] Cart page
- [ ] Mock checkout flow (3 steps)
- [ ] Order model + POST /api/orders
- [ ] Order confirmation page
- [ ] My Orders page

### Phase 5 — Admin Dashboard
- [ ] Admin-only route guard
- [ ] Dashboard stats
- [ ] Product management table
- [ ] User management table
- [ ] Order management table

### Phase 6 — Polish & Deploy
- [ ] Responsive mobile layout
- [ ] Loading states and error boundaries
- [ ] Empty states for all lists
- [ ] README with setup instructions
- [ ] Deploy (Railway for backend, Vercel for frontend)

---

## Design Principles

- Clean, minimal UI — let the products speak
- Mobile-first responsive layout
- Consistent color palette (suggest: neutral gray + a single accent color)
- Accessible: semantic HTML, focus states, alt text on all images
- Fast perceived performance: skeleton loaders while fetching

---

## Out of Scope (for this version)
- Real payment processing
- Real-time messaging between buyer/seller
- Product reviews/ratings
- Email notifications
- Social login (Google/GitHub OAuth)

These are natural next-step extensions if you want to grow the project later.
