STOCKFLOW ELECTRONICS - Node.js + Express + MySQL
==================================================

FOLDER MAP
  server.js            Backend REST API, MySQL sessions, bcrypt password hashing
  package.json         Dependencies and start commands
  database.sql         Database and tables
  seed-data.json       Sample electronics suppliers and products
  .env.example         Environment template (copy to .env; never commit .env)
  public/home.html     Home page
  public/about.html    About page
  public/contact.html  Contact page
  public/site.css      Public page styles
  public/app.html      Login, registration, dashboard and inventory features
  public/uploads/      Uploaded product photos

RUN ON YOUR LAPTOP
  1. Install Node.js 18+ and MySQL Server.
  2. Open this folder in VS Code and run: npm install
  3. Open database.sql in MySQL Workbench and execute it.
  4. Copy .env.example to .env.
  5. In .env, set DB_PASSWORD to your MySQL password.
  6. Set SESSION_SECRET to a unique random value of at least 32 characters.
  7. Before the FIRST start on an empty database, set:
       BOOTSTRAP_ADMIN_NAME=Your Name
       BOOTSTRAP_ADMIN_EMAIL=your-real-email@example.com
       BOOTSTRAP_ADMIN_PASSWORD=a-unique-password-between-10-and-72-chars
     Keep these values private. Do not share or commit your .env file.
  8. Run: npm start
  9. Open http://localhost:3000
  10. Sign in with the bootstrap administrator email and password. Public registrations
      are always created as Staff; they cannot claim administrator access.

EXISTING DATABASE / EXISTING USERS
  The server adds the auth_version column automatically when it starts. Existing sessions
  may need to sign in again after this security update.
  If your database already contains users and has no administrator, sign in to MySQL as
  the database owner and promote the account you control:
       UPDATE users SET role='admin' WHERE email='your-real-email@example.com';
  Replace the email with your own account email. Do not promote an account you do not control.

SECURITY NOTES
  - Passwords are stored as bcrypt hashes, not plain text.
  - Admin creation is controlled by private environment settings or an owner-run SQL action.
  - Current roles are checked against MySQL for protected requests.
  - Changing a password increments auth_version and ends all existing sessions for that user.
  - HTTPS secure cookies and proxy trust are enabled when NODE_ENV=production.
  - State-changing API requests require the app's custom request header.
  - Keep .env, database credentials and admin bootstrap credentials private.

DEPLOYMENT
  1. Provision a hosted MySQL database and import database.sql.
  2. Upload this project to a private GitHub repository (do not upload .env).
  3. Create a Node web service on your hosting provider and connect the repository.
  4. Build command: npm install
  5. Start command: npm start
  6. Add environment variables from .env.example in the hosting dashboard:
       NODE_ENV=production
       PORT (usually supplied automatically by the host)
       DB_HOST, DB_PORT, DB_USER, DB_PASSWORD, DB_NAME
       SESSION_SECRET (unique random secret, 32+ characters)
       BOOTSTRAP_ADMIN_NAME, BOOTSTRAP_ADMIN_EMAIL, BOOTSTRAP_ADMIN_PASSWORD
  7. Set the bootstrap admin variables only for the first start with an empty users table.
     After the admin is created, remove BOOTSTRAP_ADMIN_PASSWORD and the other bootstrap
     variables from the hosting dashboard.
  8. Confirm /api/health reports database connected, then test registration, login,
     staff permissions, admin permissions and logout before sharing the public URL.

COMMON ERRORS
  "npm is not recognized"       Install Node.js and reopen VS Code.
  "ECONNREFUSED" / "Access denied"  MySQL is stopped or DB credentials are incorrect.
  "Unknown database"            Execute database.sql.
  "SESSION_SECRET" error        Set a private secret of at least 32 characters.
  "Port 3000 in use"             Set PORT=3001 locally.
  Page not loading from file     Do not double-click HTML files; use http://localhost:3000.

OPTIONAL
  PEXELS_API_KEY       Enables optional automatic product photo lookup.
  SEED_SAMPLE_DATA=false  Skips sample suppliers and products on an empty database.
  Sample names, emails, phone numbers and GST numbers are fictional.
