# EcommerceShop Users Microservice

Manages user accounts: registration, login and profile lookup. Passwords are hashed with bcryptjs and register/login responses carry a signed JWT.

This service is one of seven Express/MongoDB microservices (auth, users, products, categories, carts, orders, favorites) that together form the backend of the EcommerceShop MERN application. Each service owns its routes under `/api/users`, connects to MongoDB through Mongoose and exposes Prometheus metrics, so it can be built, deployed, scaled and monitored independently on Kubernetes.

## API endpoints

| Method | Path | Description |
|---|---|---|
| POST | `/api/users/register` | Create a user and return it with a JWT |
| POST | `/api/users/login` | Authenticate with email and password and return a JWT |
| GET | `/api/users/profile` | Return the profile of `req.user` |
| GET | `/api/users/metrics` | Prometheus metrics |

> The profile route expects `req.user` to be resolved upstream; this service has no JWT verification middleware.

Data model: `User` (name, email, password, isAdmin, profilePicture, bio, favorites).

## Tech stack

- Node.js 22, Express 5 (ES modules)
- MongoDB with Mongoose
- prom-client (HTTP request duration histogram)
- bcryptjs, jsonwebtoken
- Docker (node:22-alpine, non-root user)

## Environment variables

Copy `.env.example` to `.env` and set your own values. Never commit `.env`.

| Variable | Description |
|---|---|
| `PORT` | HTTP port (defaults to 5000) |
| `MONGO_URI` | MongoDB connection string |
| `JWT_SECRET` | Secret used to sign JSON Web Tokens |

## Run locally

```bash
npm install
cp .env.example .env   # then edit the values
npm start
```

The service listens on `http://localhost:5000` by default.

## Docker

```bash
docker build -t ecommerceshop-users .
docker run --env-file .env -p 5000:5000 ecommerceshop-users
```

## Related repositories

- [EcommerceShop-Frontend-K8S](https://github.com/tamim-hmizi/EcommerceShop-Frontend-K8S) - frontend for the microservice deployment
- [EcommerceShop-Frontend](https://github.com/tamim-hmizi/EcommerceShop-Frontend) - frontend for the monolithic backend
- [EcommerceShop-monolithic-backend](https://github.com/tamim-hmizi/EcommerceShop-monolithic-backend) - monolithic version of the backend
- [QuickOps-Frontend](https://github.com/tamim-hmizi/QuickOps-Frontend) - QuickOps platform used to deploy this application on AKS
- Other microservices: [auth](https://github.com/tamim-hmizi/EcommerceShop-microservice-auth), [products](https://github.com/tamim-hmizi/EcommerceShop-microservice-products), [categories](https://github.com/tamim-hmizi/EcommerceShop-microservice-categories), [carts](https://github.com/tamim-hmizi/EcommerceShop-microservice-carts), [orders](https://github.com/tamim-hmizi/EcommerceShop-microservice-orders), [favorites](https://github.com/tamim-hmizi/EcommerceShop-microservice-favorites)

## License

See [LICENSE](LICENSE).

---

Part of the MERN e-commerce use case for QuickOps, 2025, by Tamim Hmizi — https://tamim-hmizi.github.io/Portfolio/
