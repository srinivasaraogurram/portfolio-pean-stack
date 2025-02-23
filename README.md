# Portfolio PEAN Stack Application

## Ideation/Story

### PEAN Application
- **Objective**: To create a personal dynamic website with a portfolio, blog, and services.
- **PEAN Stack**:
  - **P**: PostgreSQL (Database)
  - **E**: Express.js (Backend Framework)
  - **A**: Angular 18 (Frontend Framework)
  - **N**: Node.js (Runtime Environment)

---

## Application Features

### Three-Tier Architecture

#### **Tier One: Database Tier**
1. **PostgreSQL Database**:
   - Use Docker Compose to set up PostgreSQL and pgAdmin.
   - Provide instructions to initialize the schema and seed data on database startup within Docker Compose.
   - Create a Dev Container for the above setup to use with VS Code.

#### **Tier Two: Express.js (Backend)**
1. **REST API**:
   - Use Sequelize to create domain objects and manage database interactions.
2. **Domain Objects**:
   - **User**:
     - Features: Login, Logout, Session Management, Roles.
   - **Content Management**:
     - **Pages**: Add, Update, Delete, Read.
     - **Links**: Create menu links, Add, Update, Delete, Read.
     - Map links to pages.
   - **Blog Management**:
     - Create, Update, Delete, and Read blog posts.
   - **Portfolio**:
     - Align with resume.
     - Showcase projects similar to the portfolio site.
3. **Schema Design**:
   - Define the required schema, tables, and relationships using Sequelize.

#### **Tier Three: Angular 18 (Frontend)**
1. **User Interface**:
   - Create a beautiful and responsive UI based on the above features.
2. **Wireframes**:
   - Design HTML wireframes for the look and feel of the application.
3. **Functional Pages**:
   - Implement all functional pages using Angular.

---

## Step-by-Step Instructions

### **1. Database Tier Setup**
1. **Docker Compose**:
   - Create a `docker-compose.yml` file to set up PostgreSQL and pgAdmin.
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

2. **Schema and Seed Data**:
   - Create SQL scripts to initialize the schema and seed data.
   - Place these scripts in a `scripts` folder and reference them in the Docker Compose file.

3. **Dev Container**:
   - Create a `.devcontainer` folder with `Dockerfile` and `devcontainer.json` for VS Code.

---

### **2. Backend Setup (Express.js)**
1. **Initialize Express App**:
   ```bash
   mkdir backend
   cd backend
   npm init -y
   npm install express sequelize pg pg-hstore cors body-parser express-session bcrypt dotenv
   ```

2. **Sequelize Setup**:
   - Initialize Sequelize:
     ```bash
     npx sequelize-cli init
     ```
   - Define models for `User`, `Page`, `Link`, `Blog`, and `Portfolio`.

3. **REST API**:
   - Create routes and controllers for:
     - User authentication (login, logout, session management).
     - Content management (pages, links).
     - Blog management.
     - Portfolio management.

---

### **3. Frontend Setup (Angular 18)**
1. **Initialize Angular App**:
   ```bash
   ng new frontend
   cd frontend
   npm install @angular/common@latest @angular/core@latest
   ```

2. **Create Components**:
   - Generate components for:
     - Login, Register, Profile.
     - Pages, Links, Blog, Portfolio.

3. **Wireframes**:
   - Design wireframes using tools like Figma or Sketch.

4. **Functional Pages**:
   - Implement pages using Angular components and services.

---

### **4. Integration**
1. **Connect Frontend to Backend**:
   - Use Angular's `HttpClient` to interact with the Express backend.

2. **Session Management**:
   - Use Express sessions for authentication and session management.

3. **Deployment**:
   - Deploy the application using platforms like Heroku, Vercel, or AWS.

---

## Final Notes
- Ensure proper error handling and validation at both the backend and frontend.
- Use environment variables for sensitive information.
- Test the application thoroughly before deployment.

Let me know if you need further assistance!

Refer to [Soltion](./PEAN-Solution.md)
