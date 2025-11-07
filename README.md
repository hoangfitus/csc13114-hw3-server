# User Registration System - Backend

A NestJS backend API for user registration and authentication with PostgreSQL and TypeORM.

## Prerequisites

- Node.js (v20 or higher)
- PostgreSQL (v12 or higher)
- npm or yarn

## Tech Stack

- **Framework**: NestJS
- **Database**: PostgreSQL
- **ORM**: TypeORM
- **Validation**: class-validator, class-transformer
- **Password Hashing**: bcrypt

## Installation

1. Install dependencies:

```bash
npm install
```

2. Set up environment variables:

Create a `.env` file in the backend directory (you can copy from `.env.example`):

```bash
cp .env.example .env
```

Configure the following variables in `.env`:

```env
# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=postgres
DB_DATABASE=user_registration

# Application
PORT=3000
NODE_ENV=development
```

3. Set up PostgreSQL database:

Create a database named `user_registration` (or the name you specified in `.env`):

```bash
# Connect to PostgreSQL
psql -U postgres

# Create database
CREATE DATABASE user_registration;

# Exit psql
\q
```

## Running the Application

### Development mode (with hot reload)

```bash
npm run start:dev
```

The API will be available at `http://localhost:3000`

### Production mode

```bash
# Build the application
npm run build

# Run in production
npm run start:prod
```

## API Endpoints

### POST `/user/register`

Register a new user.

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Success Response (201):**

```json
{
  "message": "User registered successfully",
  "user": {
    "id": "uuid-here",
    "email": "user@example.com",
    "createdAt": "2025-11-07T10:30:00.000Z"
  }
}
```

**Error Responses:**

- **400 Bad Request**: Invalid input (e.g., invalid email format, password too short)

```json
{
  "statusCode": 400,
  "message": [
    "Email is required",
    "Password must be at least 8 characters long"
  ],
  "error": "Bad Request"
}
```

- **409 Conflict**: Email already exists

```json
{
  "statusCode": 409,
  "message": "Email already exists",
  "error": "Conflict"
}
```

### POST `/user/login`

Authenticate an existing user.

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Success Response (200):**

```json
{
  "message": "Login successful",
  "user": {
    "id": "uuid-here",
    "email": "user@example.com",
    "createdAt": "2025-11-07T10:30:00.000Z"
  }
}
```

**Error Responses:**

- **400 Bad Request**: Invalid input (e.g., invalid email format, password too short)

```json
{
  "statusCode": 400,
  "message": [
    "Email is required",
    "Password must be at least 8 characters long"
  ],
  "error": "Bad Request"
}
```

- **401 Unauthorized**: Invalid credentials

```json
{
  "statusCode": 401,
  "message": "Invalid email or password",
  "error": "Unauthorized"
}
```

## Validation Rules

- **Email**: Required, must be a valid email format
- **Password**: Required, minimum 8 characters

## Project Structure

```
src/
├── user/
│   ├── dto/
│   │   ├── register-user.dto.ts    # Registration DTO with validation
│   │   └── login-user.dto.ts       # Login DTO with validation
│   ├── user.controller.ts          # API endpoints (register, login)
│   ├── user.entity.ts              # User entity definition
│   ├── user.module.ts              # User module
│   └── user.service.ts             # Business logic (register, login)
├── app.module.ts                   # Root module
└── main.ts                         # Application entry point
```

## Testing

```bash
# Unit tests
npm run test

# e2e tests
npm run test:e2e

# Test coverage
npm run test:cov
```

## Development

### Linting

```bash
npm run lint
```

### Formatting

```bash
npm run format
```

## Security Features

- Passwords are hashed using bcrypt with 10 salt rounds
- Password verification using bcrypt.compare for login
- CORS is enabled for frontend integration
- Input validation using class-validator on all endpoints
- SQL injection protection via TypeORM
- Generic error messages for failed login attempts (security best practice)

## Notes

- TypeORM `synchronize` is set to `true` for development. **Set it to `false` in production** and use migrations instead.
- The application automatically creates database tables on startup based on entity definitions.

## License

UNLICENSED
