# Authentication App with Secrets Management

This is a Node.js application that demonstrates user authentication using `passport` with local and Google OAuth2 strategies. Authenticated users can log in, register, and submit secrets that are stored in a PostgreSQL database.

## Features

- User registration and login using:
  - Local authentication (email and password)
  - Google OAuth2 authentication
- Password hashing using `bcrypt` for security.
- Secrets submission and display functionality for authenticated users.
- Session-based authentication using `express-session`.

## Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/) (v14 or higher)
- [PostgreSQL](https://www.postgresql.org/) (v12 or higher)
- npm (comes with Node.js)

## Installation

1. Clone the repository:

   ```bash
   git clone <repository-url>
   ```

2. Navigate to the project directory:

   ```bash
   cd authentication-app
   ```

3. Install dependencies:

   ```bash
   npm install
   ```

4. Create a `.env` file in the root directory with the following variables:

   ```env
   PG_USER=your_postgresql_user
   PG_HOST=localhost
   PG_DATABASE=your_database_name
   PG_PASSWORD=your_database_password
   PG_PORT=5432

   SESSION_SECRET=your_session_secret
   GOOGLE_CLIENT_ID=your_google_client_id
   GOOGLE_CLIENT_SECRET=your_google_client_secret
   ```

5. Set up the PostgreSQL database:
   - Create a database and a table named `secrets`:
     ```sql
     CREATE TABLE secrets (
       id SERIAL PRIMARY KEY,
       email VARCHAR(255) NOT NULL UNIQUE,
       password VARCHAR(255),
       secret TEXT
     );
     ```

## Running the Application

1. Start the PostgreSQL server.

2. Start the Node.js application:

   ```bash
   node app.js
   ```

3. Open your browser and navigate to:

   ```
   http://localhost:3000
   ```

## Application Workflow

### 1. Routes Overview

#### Public Routes:
- **`GET /`**  
  Renders the homepage.
  
- **`GET /login`**  
  Renders the login page.

- **`GET /register`**  
  Renders the registration page.

#### Authentication Routes:
- **`POST /register`**  
  Registers a new user with email and password, hashes the password, and stores it in the database.

- **`POST /login`**  
  Authenticates a user using email and password with the local strategy.

- **`GET /auth/google`**  
  Initiates Google OAuth2 login.

- **`GET /auth/google/secrets`**  
  Handles the callback from Google OAuth2, redirects to `/secrets` on success.

- **`GET /logout`**  
  Logs out the user and redirects to the homepage.

#### Protected Routes:
- **`GET /secrets`**  
  Displays the secret submitted by the logged-in user.

- **`GET /submit`**  
  Renders a form where authenticated users can submit a secret.

- **`POST /submit`**  
  Updates the user's secret in the database.

### 2. Authentication Logic

- Passwords are hashed using `bcrypt` before storing in the database.
- Sessions are managed using `express-session`, and user authentication is persisted across requests.
- Google OAuth2 allows users to log in without registering locally.

### 3. Database Structure

| Column   | Type        | Description                    |
|----------|-------------|--------------------------------|
| `id`     | SERIAL      | Primary key.                  |
| `email`  | VARCHAR(255)| Unique email for the user.    |
| `password`| VARCHAR(255)| Hashed password for local auth.|
| `secret` | TEXT        | Secret submitted by the user. |

## Example `.env` File

```env
PG_USER=admin
PG_HOST=localhost
PG_DATABASE=secrets_db
PG_PASSWORD=admin123
PG_PORT=5432

SESSION_SECRET=supersecretkey
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
```

## Example Usage

### Test API with curl or Postman

#### Register a New User:
```bash
curl -X POST -d "username=test@example.com&password=123456" http://localhost:3000/register
```

#### Login:
```bash
curl -X POST -d "username=test@example.com&password=123456" http://localhost:3000/login
```

#### Submit a Secret:
```bash
curl -X POST -d "secret=My new secret" http://localhost:3000/submit
```

## Dependencies

- `express`: Web framework.
- `body-parser`: Parses incoming requests.
- `pg`: PostgreSQL client for Node.js.
- `bcrypt`: Hashing library for password security.
- `passport`: Authentication middleware.
- `passport-local`: Local strategy for passport.
- `passport-google-oauth2`: Google OAuth2 strategy for passport.
- `express-session`: Session management middleware.
- `dotenv`: Loads environment variables from `.env` file.
