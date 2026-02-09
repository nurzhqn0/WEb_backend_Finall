# Coffee Menu & Ordering Web App (Backend + MongoDB)

This is a full-stack final project:
- **Backend:** Node.js + Express
- **Database:** MongoDB (Mongoose)
- **Frontend:** Static HTML/JS (served from `public/`)

## 1) Setup (Local)

### Requirements
- Node.js 18+
- MongoDB local (or MongoDB Atlas)

### Install
```bash
npm install
cp .env.example .env
```

Edit `.env` and set:
- `MONGO_URI`
- `JWT_SECRET`
- `ADMIN_EMAIL` / `ADMIN_PASSWORD`
- SMTP settings (optional, for Contact page)

### Run
```bash
npm run dev
```

Open: `http://localhost:3000`

## 2) Products

Products are stored **only in MongoDB**.

Insert them via MongoDB Compass or `mongosh` using `insertMany([...])`.

## 3) Frontend pages
- `/index.html` Home
- `/menu.html` Menu (loads products from MongoDB)
- `/cart.html` Cart (saved in MongoDB per user)
- `/profile.html` Profile
- `/admin-login.html` Admin login (env credentials)
- `/admin.html` Admin panel (products + orders + analytics)

## 4) API Endpoints

### Health
- `GET /api/health`

### Auth (public)
- `POST /api/auth/register`
- `POST /api/auth/login`

### Users (private)
- `GET /api/users/me`
- `PUT /api/users/me`

### Admin (public)
- `POST /api/admin/login` (checks env credentials, returns admin JWT)

### Products
- `GET /api/products` (supports `q`, `type`, `available`, `page`, `limit`)
- `GET /api/products/:id`
- `POST /api/products` (admin)
- `PUT /api/products/:id` (admin)
- `DELETE /api/products/:id` (admin)

### Cart (private)
- `GET /api/cart`
- `POST /api/cart/items` (add, uses `$inc` / `$push`)
- `PATCH /api/cart/items/:id` (update, uses positional `$set`)
- `DELETE /api/cart/items/:id` (remove, uses `$pull`)
- `DELETE /api/cart/clear`

### Orders
- `POST /api/orders` (private, creates order from cart)
- `GET /api/orders` (private, my orders)
- `GET /api/orders/:id` (private)
- `GET /api/orders/admin/all` (admin)
- `PATCH /api/orders/admin/:id/status` (admin)

### Contact
- `POST /api/contact/send` (SMTP via Nodemailer)

### Analytics (Aggregation) (admin)
- `GET /api/analytics/top-products`
- `GET /api/analytics/sales-by-status`

## 5) Notes
- Products use **embedded** `sizeOptions`, carts use **embedded** `items`, orders store a snapshot of items.
- Aggregation endpoints are multi-stage pipelines on orders.
- Indexes are defined in Mongoose schemas for common queries.
