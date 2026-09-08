# Animal Adoption Platform

A web-based animal adoption application built with Node.js, Express, and EJS. The app supports guest browsing and role-based access for administrators, and is configured to run against either a local **MySQL** database or an **Azure SQL Server** database.

## 🚀 Features

- Guest users can browse animals and most application features without the ability to adopt or cancel an adoption
- Admin users can cancel adoptions, add new temperaments, and add new species — access controlled via user roles
- Database population via a one-click "Populate" button that seeds tables from predefined JSON files
- Service-layer architecture (`AnimalService`, `SpeciesService`, `TemperamentService`, `UserService`) separating business logic from routes
- Sequelize associations modeling a many-to-many relationship between animals and temperaments, and belongs-to relationships for species and adopter

## 🛠 Tech Stack

- **Backend:** Node.js, Express
- **Templating:** EJS
- **ORM:** Sequelize
- **Database:** MySQL (local) or **Azure SQL Server** (cloud) — note: the two environments use *different* database engines, not the same one in two locations
- **Authentication:** Passport (passport-local strategy)
- **Sessions:** express-session with connect-sqlite3 store
- **Other:** dotenv, connect-flash, SweetAlert (UI alerts)

## ☁️ Environment Configuration

This project supports two deployment targets, each with its own environment file **template**:

- **`envlocal.sample`** — configuration for running against a local MySQL instance
- **`envAzure.sample`** — configuration for running against an Azure SQL Server database

Copy the relevant template to `.env` at the project root, fill in your **own** credentials, and ensure `require('dotenv').config();` is included in the app's entry file to load the configuration.

> ⚠️ **Security note:** Never commit a filled-in `.env`, `envlocal`, or `envAzure` file with real credentials. Both are listed in `.gitignore`. If you're setting this project up from a clone that predates this note, rotate any credentials that may have been exposed and regenerate them before use.

### Required environment variables

**Local (MySQL):**
```env
ADMIN_USERNAME=<your-mysql-username>
ADMIN_PASSWORD=<your-mysql-password>
DATABASE_NAME="adoptiondb"
DIALECT="mysql"
DIALECTMODEL="mysql2"
PORT="3000"
HOST="localhost"
```

**Azure (SQL Server):**
```env
ADMIN_USERNAME=<your-azure-sql-username>
ADMIN_PASSWORD=<your-azure-sql-password>
DATABASE_NAME="adoptiondb"
DIALECT="mssql"
HOST="<your-server-name>.database.windows.net"
PORT="1433"
ENCRYPT=true
TRUST_SERVER_CERTIFICATE=false
```

## 📦 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/devrimsavas/Animal-Adopt-Azure.git
   cd Animal-Adopt-Azure
   ```
2. **Install dependencies**
   ```bash
   npm install
   ```
3. **Set up environment variables** — copy one of the sample env files to `.env` and fill in your own credentials (see above)
4. **Database setup** — `app.js` calls:
   ```js
   db.sequelize.sync({ force: true }).then(() => {
     console.log('Database and tables created!');
   });
   ```
   ⚠️ This drops and recreates all existing tables on every start — use with caution, and disable `force: true` once you have real data you want to keep.

## ▶️ Usage

1. **Start the application**
   ```bash
   npm start
   ```
   The server launches on `localhost:3000` (or your configured Azure endpoint).
2. **Populate the database** — on first run, navigate to the main page and press **"Populate"** to create and seed the database tables from predefined JSON files.
3. **Browse as a guest** — view animals and most features without adopting.
4. **Manage as an admin** — cancel adoptions, add temperaments and species via role-based access.

## 🗄️ Database Setup (MySQL, local)

```sql
CREATE DATABASE adoptiondb;
GRANT ALL PRIVILEGES ON adoptiondb.* TO 'your_user'@'localhost';
FLUSH PRIVILEGES;
```

For the Azure setup, create the equivalent database on your Azure SQL Server instance and update your `.env` accordingly.

## 📋 Requirements

- Node.js **v20.10.0** or higher (check with `node -v`)
- MySQL (local instance) **or** an Azure SQL Server database

## 📝 Known Limitations

- **Passwords are compared in plain text** in the current Passport strategy (`passport-config.js`), not hashed with bcrypt despite it being a listed dependency. This is a known gap from the project's early development and would need to be fixed (using `bcrypt.compare` against a hashed password) before any real deployment.
- `connect-flash` is included for future user-feedback improvements but not yet wired into every flow.

## 📝 Notes

This is a learning project built to practice full-stack development with dual local/cloud deployment configurations across two different database engines (MySQL and Azure SQL Server).
