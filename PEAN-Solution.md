Creating a complete codebase for the **Portfolio PEAN Stack Application** as described is a large task. Below, I’ll provide a structured implementation for each tier (**Database**, **Backend**, and **Frontend**) with step-by-step instructions and code snippets. This will serve as a foundation for your application.

---

## **1. Database Tier**

### **Docker Compose Setup**
Create a `docker-compose.yml` file to set up PostgreSQL and pgAdmin:

```yaml
version: '3.8'
services:
  db:
    image: postgres:latest
    container_name: postgres_db
    environment:
      POSTGRES_USER: your_db_user
      POSTGRES_PASSWORD: your_db_password
      POSTGRES_DB: portfolio_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "5050:80"
    depends_on:
      - db

volumes:
  postgres_data:
```

### **Schema and Seed Data**
Create SQL scripts to initialize the schema and seed data. Save these in a `scripts` folder.

1. **Schema Initialization** (`scripts/init.sql`):
   ```sql
   CREATE TABLE users (
       id SERIAL PRIMARY KEY,
       username VARCHAR(50) UNIQUE NOT NULL,
       password VARCHAR(255) NOT NULL,
       email VARCHAR(100) UNIQUE NOT NULL,
       role VARCHAR(50) DEFAULT 'user',
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE pages (
       id SERIAL PRIMARY KEY,
       title VARCHAR(100) NOT NULL,
       content TEXT NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE links (
       id SERIAL PRIMARY KEY,
       name VARCHAR(100) NOT NULL,
       url VARCHAR(255) NOT NULL,
       page_id INT REFERENCES pages(id) ON DELETE CASCADE,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE blogs (
       id SERIAL PRIMARY KEY,
       title VARCHAR(100) NOT NULL,
       content TEXT NOT NULL,
       user_id INT REFERENCES users(id) ON DELETE CASCADE,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE portfolios (
       id SERIAL PRIMARY KEY,
       project_name VARCHAR(100) NOT NULL,
       description TEXT NOT NULL,
       user_id INT REFERENCES users(id) ON DELETE CASCADE,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```

2. **Seed Data** (`scripts/seed.sql`):
   ```sql
   INSERT INTO users (username, password, email, role) VALUES
   ('admin', 'hashed_password', 'admin@example.com', 'admin'),
   ('user1', 'hashed_password', 'user1@example.com', 'user');

   INSERT INTO pages (title, content) VALUES
   ('Home', 'Welcome to the home page!'),
   ('About', 'This is the about page.');

   INSERT INTO links (name, url, page_id) VALUES
   ('Home Link', '/home', 1),
   ('About Link', '/about', 2);

   INSERT INTO blogs (title, content, user_id) VALUES
   ('First Blog', 'This is the first blog post.', 1),
   ('Second Blog', 'This is the second blog post.', 2);

   INSERT INTO portfolios (project_name, description, user_id) VALUES
   ('Project 1', 'This is project 1.', 1),
   ('Project 2', 'This is project 2.', 2);
   ```

3. **Update Docker Compose** to run the scripts:
   ```yaml
   db:
     image: postgres:latest
     container_name: postgres_db
     environment:
       POSTGRES_USER: your_db_user
       POSTGRES_PASSWORD: your_db_password
       POSTGRES_DB: portfolio_db
     ports:
       - "5432:5432"
     volumes:
       - postgres_data:/var/lib/postgresql/data
       - ./scripts:/docker-entrypoint-initdb.d
   ```

---

## **2. Backend Tier (Express.js)**

### **Project Setup**
1. Initialize the project:
   ```bash
   mkdir backend
   cd backend
   npm init -y
   npm install express sequelize pg pg-hstore cors body-parser express-session bcrypt dotenv
   ```

2. **Sequelize Setup**:
   Initialize Sequelize:
   ```bash
   npx sequelize-cli init
   ```

3. **Database Configuration** (`config/config.json`):
   ```json
   {
     "development": {
       "username": "your_db_user",
       "password": "your_db_password",
       "database": "portfolio_db",
       "host": "localhost",
       "dialect": "postgres"
     }
   }
   ```

4. **Define Models**:
   - `User` model (`models/user.js`):
     ```javascript
     module.exports = (sequelize, DataTypes) => {
       const User = sequelize.define('User', {
         username: { type: DataTypes.STRING, unique: true, allowNull: false },
         password: { type: DataTypes.STRING, allowNull: false },
         email: { type: DataTypes.STRING, unique: true, allowNull: false },
         role: { type: DataTypes.STRING, defaultValue: 'user' }
       });
       return User;
     };
     ```
   - Repeat for `Page`, `Link`, `Blog`, and `Portfolio`.

5. **REST API**:
   - Create routes and controllers for:
     - User authentication (login, logout, session management).
     - Content management (pages, links).
     - Blog management.
     - Portfolio management.

---

## **3. Frontend Tier (Angular 18)**

### **Project Setup**
1. Initialize Angular app:
   ```bash
   ng new frontend
   cd frontend
   npm install @angular/common@latest @angular/core@latest
   ```

2. **Create Components**:
   Generate components for:
   ```bash
   ng generate component login
   ng generate component register
   ng generate component profile
   ng generate component pages
   ng generate component blogs
   ng generate component portfolio
   ```

3. **Wireframes**:
   Design wireframes using tools like Figma or Sketch.

4. **Functional Pages**:
   Implement pages using Angular components and services.

---

## **4. Integration**
1. **Connect Frontend to Backend**:
   Use Angular's `HttpClient` to interact with the Express backend.

2. **Session Management**:
   Use Express sessions for authentication and session management.

3. **Deployment**:
   Deploy the application using platforms like Heroku, Vercel, or AWS.

---

This is a high-level implementation. Let me know if you need detailed code for any specific part!
