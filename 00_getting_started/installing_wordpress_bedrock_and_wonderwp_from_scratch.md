# Installing WordPress, Bedrock, and WonderWp from scratch (scripted)

If you want to start a **modern WordPress project rapidly** with:

✅ [**Composer**](https://getcomposer.org/) as a package manager\
✅ [**Bedrock**](https://roots.io/bedrock/) as a WordPress boilerplate\
✅ [**WP-CLI**](https://wp-cli.org/) as a command-line tool\
✅ [**WonderWP**](../) as a development framework

Then follow the steps below to **set up your environment quickly and efficiently**.

***

### 1️⃣ Create a New Project

Run the following command to **install Bedrock and navigate into the project folder**:

```bash
composer create-project roots/bedrock my-wonderwp-project && cd my-wonderwp-project
```

***

### 2️⃣ Configure Your Environment Variables

Edit the **`.env` file** inside your project and update the following variables:

#### **🔹 Database Configuration**

```
DB_NAME=your_database_name
DB_USER=your_database_user
DB_PASSWORD=your_database_password
DB_HOST=127.0.0.1
```

🔹 _Alternatively, you can use a DSN string:_

```
DATABASE_URL=mysql://user:password@127.0.0.1:3306/db_name
```

#### **🔹 WordPress Configuration**

```
WP_ENV=development  # Set to development, staging, or production
WP_HOME=https://example.com
WP_SITEURL=${WP_HOME}/wp
```

#### **🔹 Security Keys**

Generate unique authentication salts:

* **With WP-CLI**:

```
wp dotenv salts regenerate
```

* **Or use the** [**official WordPress salt generator**](https://api.wordpress.org/secret-key/1.1/salt/)

***

### 3️⃣ Install WP-CLI

To use **WP-CLI**, install it via Composer:

```bash
composer require wp-cli/wp-cli 
composer require wp-cli/wp-cli-bundle
```

***

### 4️⃣ Install WordPress

Run the WordPress installation command with your credentials:

```bash
vendor/bin/wp core install
--url="yourlocalurl"
--title="YourSiteTitle"
--admin_user="youradminuser"
--admin_email="youradmin@example.com"
```

***

### 5️⃣ Install WonderWP

WonderWP is available as a Composer package. Install it by running:

```bash
composer require wonderwp/wonderwp
```

***

### 6️⃣ Configure Your Web Server

Set your web server’s **document root** to Bedrock’s `web/` folder: `/path/to/site/web/`

***

### ✅ Final Steps

1️⃣ **Visit your site:** [https://example.com/wp/wp-admin/](https://example.com/wp/wp-admin/)\
2️⃣ **Log in to your admin panel:** [https://example.com/wp/wp-admin/](https://example.com/wp/wp-admin/)\
3️⃣ **Verify that WordPress and WonderWP are properly installed.**

🎉 _With just a few commands, your project is now set up with full modern development capabilities!_

📌 **Next Step: Create Your First Plugin →**
