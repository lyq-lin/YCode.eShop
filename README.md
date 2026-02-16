# YCode.eShop

A full-stack e-commerce learning project with Vue.js frontend and .NET microservices backend.

## Project Overview

YCode.eShop is a comprehensive e-commerce application built using modern web technologies. The project demonstrates a microservices architecture with separate services for different business domains, providing a practical example of building scalable e-commerce systems.

### Architecture

```
YCode.eShop/
├── frontend/          # Vue.js 3 SPA application
└── backend/           # .NET 8 microservices
    ├── Product.WebAPI       # Product catalog management
    ├── User.WebAPI          # User authentication and management
    ├── Cart.WebAPI          # Shopping cart functionality
    ├── Order.WebAPI         # Order processing and management
    ├── Payment.WebAPI       # Payment integration (Alipay)
    ├── Common.Jwt           # JWT authentication shared library
    ├── Common.RabbitMQ      # Message queue integration
    └── Common.Alipay        # Alipay SDK integration
```

## Tech Stack

### Frontend
- **Framework**: Vue.js 3
- **Build Tool**: Vue CLI
- **State Management**: Pinia
- **Routing**: Vue Router 4
- **UI Library**: Element Plus
- **HTTP Client**: Axios
- **Authentication**: JWT (jwt-decode)

### Backend
- **Framework**: .NET 8 / ASP.NET Core Web API
- **Architecture**: Microservices with Domain-Driven Design (DDD)
- **Authentication**: JWT Bearer
- **Message Queue**: RabbitMQ
- **Payment**: Alipay SDK
- **Database**: Entity Framework Core (SQL Server)
- **API Documentation**: Swagger/OpenAPI

## Features

### User Management
- User registration with phone number and SMS verification
- User login with JWT token authentication
- User profile management
- Access control and login history tracking

### Product Management
- Product catalog with categories and types
- Product variants support
- Product search functionality
- Featured products display

### Shopping Cart
- Add/remove items from cart
- Update item quantities
- Cart persistence

### Order Management
- Create orders from cart
- Order history and tracking
- Order status management

### Payment Integration
- Alipay payment integration
- Payment callback handling
- Payment status tracking

## Prerequisites

### Frontend
- Node.js 16+ and npm
- Vue CLI 5.0+

### Backend
- .NET 8 SDK
- SQL Server
- RabbitMQ Server
- Alipay Developer Account (for payment integration)
- Aliyun SMS Account (for phone verification)

## Installation

### Backend Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/lyq-lin/YCode.eShop.git
   cd YCode.eShop/backend
   ```

2. **Configure Aliyun SMS**
   - Open `User.WebAPI/appsettings.json` (or `appsettings.Development.json`)
   - Add your Aliyun SMS access credentials:
   ```json
   {
     "SmsSettings": {
       "AccessKey": "your-access-key",
       "AccessKeySecret": "your-secret-key",
       "SignName": "your-sign-name",
       "TemplateCode": "your-template-code"
     }
   }
   ```

3. **Configure Alipay**
   - Open `Payment.WebAPI/appsettings.json` (or `appsettings.Development.json`)
   - Add your Alipay application credentials:
   ```json
   {
     "AlipaySettings": {
       "AppId": "your-app-id",
       "PrivateKey": "your-private-key",
       "PublicKey": "your-public-key",
       "ServerUrl": "https://openapi.alipay.com/gateway.do"
     }
   }
   ```

4. **Configure RabbitMQ**
   - Ensure RabbitMQ server is running
   - Configure connection string in `appsettings.json`:
   ```json
   {
     "RabbitMQSettings": {
       "HostName": "localhost",
       "Port": 5672,
       "UserName": "guest",
       "Password": "guest"
     }
   }
   ```

5. **Configure Database Connections**
   - Update connection strings for each service in their respective `appsettings.json`:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=localhost;Database=YCodeShop;Trusted_Connection=True;TrustServerCertificate=True;"
     }
   }
   ```

6. **Run Database Migrations**
   ```bash
   # Navigate to each Infrastructure project and run migrations
   cd Product.Infrastructure
   dotnet ef database update

   cd ../User.Infrastructure
   dotnet ef database update

   cd ../Cart.Infrastructure
   dotnet ef database update

   cd ../Order.Infrastructure
   dotnet ef database update
   ```

7. **Start the Services**
   ```bash
   # Start each WebAPI project
   cd ../Product.WebAPI && dotnet run
   # In a new terminal:
   cd ../User.WebAPI && dotnet run
   # Repeat for Cart, Order, and Payment services
   ```

### Frontend Setup

1. **Navigate to frontend directory**
   ```bash
   cd ../frontend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure API Base URL**
   - Locate the API base URL configuration in the stores files (e.g., `src/stores/useProduct.js`)
   - Update the IP address to match your backend server
   - Example: Change `http://43.143.170.48:8080` to your server address

4. **Run development server**
   ```bash
   npm run serve
   ```

   The application will be available at `http://localhost:8080`

5. **Build for production**
   ```bash
   npm run build
   ```

## API Documentation

Each microservice includes Swagger/OpenAPI documentation. Access the documentation at:

- **Product API**: `http://localhost:5000/swagger`
- **User API**: `http://localhost:5001/swagger`
- **Cart API**: `http://localhost:5002/swagger`
- **Order API**: `http://localhost:5003/swagger`
- **Payment API**: `http://localhost:5004/swagger`

*Note: Ports may vary based on your configuration.*

## Project Structure

### Frontend Structure

```
frontend/
├── public/              # Static assets
├── src/
│   ├── assets/         # Images, logos, etc.
│   ├── components/     # Vue components
│   │   ├── CartCounter.vue
│   │   ├── FeaturedProduct.vue
│   │   ├── LogoButton.vue
│   │   ├── Nav.vue
│   │   ├── ProductList.vue
│   │   ├── Search.vue
│   │   └── UserButton.vue
│   ├── pages/          # Page components
│   │   ├── Cart/
│   │   ├── Home/
│   │   ├── Login/
│   │   ├── Order/
│   │   ├── OrderDetail/
│   │   ├── PaymentSuccess/
│   │   ├── ProductDetail/
│   │   ├── Profile/
│   │   └── Register/
│   ├── stores/         # Pinia stores
│   │   ├── useCart.js
│   │   ├── useCategory.js
│   │   ├── useOrder.js
│   │   ├── usePayment.js
│   │   ├── useProductList.js
│   │   └── useUser.js
│   ├── router/         # Vue Router configuration
│   ├── hook/           # Custom hooks
│   ├── App.vue         # Root component
│   └── main.js         # Application entry point
├── package.json
└── vue.config.js
```

### Backend Structure

Each microservice follows Domain-Driven Design (DDD) principles:

```
backend/
├── [Domain].WebAPI/           # API layer
│   ├── Controllers/          # REST API controllers
│   ├── Program.cs            # Application entry point
│   └── Dockerfile
├── [Domain].Domain/          # Domain layer
│   ├── Entity/               # Domain entities
│   ├── I[Entity]Repository.cs # Repository interfaces
│   └── [Domain]DomainService.cs
└── [Domain].Infrastructure/  # Infrastructure layer
    ├── DbContexts/           # EF Core DbContext
    ├── Migrations/           # Database migrations
    ├── Configs/              # Entity configurations
    └── [Entity]Repository.cs # Repository implementations
```

## Database Schema

The project uses separate databases for each bounded context:
- **ProductDB**: Products, categories, product types, product variants
- **UserDB**: Users, user access failures, login history
- **CartDB**: Cart items
- **OrderDB**: Orders, order items

Seed data is provided in `backend/SeedData.sql`.

## Security Considerations

- JWT-based authentication with version tracking
- Phone number verification via SMS
- Access failure tracking for security
- API rate limiting (implementation recommended)

## Docker Support

Each microservice includes a Dockerfile for containerized deployment:

```bash
# Build Docker images
docker build -t ycodeshop-product-api backend/Product.WebAPI
docker build -t ycodeshop-user-api backend/User.WebAPI
# ... etc.

# Run containers
docker run -p 5000:8080 ycodeshop-product-api
# ... etc.
```

## Contributing

This is a learning project. Feel free to fork and experiment!

## License

This project is for educational purposes only.

## Acknowledgments

- Built with Vue.js and ASP.NET Core
- Inspired by modern e-commerce systems
- Thanks to the open-source community

---

**Note**: This is a learning project. For production use, additional security measures, error handling, and performance optimizations should be implemented.
