# Animal Adoption Platform

A web-based animal adoption application built with Node.js, Express, and EJS. The app supports guest browsing and role-based access for administrators, and is configured to run against either a local MySQL database or an Azure-hosted MySQL database.

## 🚀 Features

- Guest users can browse animals and most application features without the ability to adopt or cancel an adoption
- Admin users can cancel adoptions, add new temperaments, and add new species — access controlled via user roles
- Database population via a one-click "Populate" button that seeds tables from predefined JSON files

## 🛠 Tech Stack

- **Backend:** Node.js, Express
- **Templating:** EJS
- **ORM:** Sequelize
- **Database:** MySQL (local or **Azure-hosted**)
- **Authentication:** Passport (passport-local strategy)
- **Sessions:** express-session with connect-sqlite3 store
- **Other:** dotenv, bcrypt, connect-flash, SweetAlert (UI alerts)

## ☁️ Environment Configuration

This project supports two deployment targets, each with its own environment file:

- **`envlocal`** — configuration for running against a local MySQL instance
- **`envAzure`** — configuration for running against an Azure-hosted MySQL database

Copy the relevant file to `.env` at the project root depending on which environment you want to run against, and ensure `require('dotenv').config();` is included in the app's entry file (`www`) to load these configurations.

### Required environment variables

```env
ADMIN_USERNAME="dabcaowner"
ADMIN_PASSWORD="dabca1234"
DATABASE_NAME="adoptiondb"
DIALECT="mysql"
DIALECTMODEL="mysql2"
PORT="3000"
HOST="localhost"   # or your Azure MySQL host when using envAzure
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
3. **Set up environment variables** — copy either `envlocal` or `envAzure` to `.env` and fill in your database credentials
4. **Database setup** — ensure `app.js` includes:
   ```js
   db.sequelize.sync({ force: true }).then(() => {
     console.log('Database and tables created!');
   });
   ```
   ⚠️ This drops and recreates all existing tables — use with caution.

## ▶️ Usage

1. **Start the application**
   ```bash
   npm start
   ```
   The server launches on `localhost:3000` (or the configured Azure endpoint).
2. **Populate the database** — on first run, navigate to the main page and press **"Populate"** to create and seed the database tables from predefined JSON files.
3. **Browse as a guest** — view animals and most features without adopting.
4. **Manage as an admin** — cancel adoptions, add temperaments and species via role-based access.

## 🗄️ Database Setup (MySQL)

Create the database:
```sql
CREATE DATABASE adoptiondb;
```

Create a user and grant permissions:
```sql
GRANT ALL PRIVILEGES ON adoptiondb.* TO 'dabcaowner'@'localhost';
FLUSH PRIVILEGES;
```

For the Azure-hosted setup, create the equivalent database and user on your Azure Database for MySQL instance and update `envAzure` accordingly.

## 📋 Requirements

- Node.js **v20.10.0** or higher (check with `node -v`)
- MySQL (local instance or Azure Database for MySQL)

## 📝 Notes

- `bcrypt` and `connect-flash` are included as dependencies for future development (enhanced security and user feedback). Authentication currently uses **Passport** with a simple username/password strategy.
- This is a learning project built to practice full-stack development with dual local/cloud deployment configurations.
