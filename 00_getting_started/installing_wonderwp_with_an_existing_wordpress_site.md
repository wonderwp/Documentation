# Installing WonderWp on an existing WordPress install

## Installing WonderWP on an Existing WordPress Install

WonderWP relies on **Composer** to run properly. This guide will walk you through **setting up Composer in WordPress** and **installing WonderWP** step by step.

***

### 1️⃣ Prepare Your WordPress Install for Composer

#### **1.1 Check Your `composer.json` File**

To ensure your WordPress site supports Composer, you need a valid **`composer.json`** file in your project root.

If you don’t already have one, create it manually with the following content:

```json
{
    "name": "yourauthorname/yourprojectname",
    "description": "Testing a WordPress project with WonderWP capabilities",
    "extra": {
        "installer-paths": {
            "wp-content/mu-plugins/{$name}/": [
                "type:wordpress-muplugin"
            ],
            "wp-content/plugins/{$name}/": [
                "type:wordpress-plugin"
            ],
            "wp-content/themes/{$name}/": [
                "type:wordpress-theme"
            ]
        },
        "wordpress-install-dir": "/"
    }
}
```



🔹 **Replace** `"yourauthorname/yourprojectname"` with your actual values.\
🔹 If you already have a **`composer.json`**, ensure the **`extra`** section is included and adjust paths as needed.

***

#### **1.2 Install Composer Dependencies**

To ensure Composer can handle WordPress installations correctly, install the **Composer Installers package**:

```bash
composer require composer/installers
```



***

#### **1.3 Verify Composer is Ready**

After these steps, Composer should:\
✅ **Recognize your WordPress installation directory.**\
✅ **Know where to install themes, plugins, and must-use plugins.**

At this point, you can start managing dependencies through Composer.

***

### 2️⃣ Install WonderWP

WonderWP is available as a **Composer package**. To install it, run:

```bash
composer require wonderwp/wonderwp
```

#### **Expected Outcome**

After installation, you should see the following folders in your project:

```
vendor/wonderwp/               # WonderWP package inside vendor
wp-content/mu-plugins/autoload-wwp/   # WonderWP autoload plugin
wp-content/mu-plugins/generator-wwp/  # WonderWP generator plugin
```



⚠️ **If these folders are missing**, check your **Composer configuration**, especially the **installer paths** in `composer.json`.

***

### 3️⃣ Initialize WonderWP

#### **Ensure the Framework Loader is Called**

By default, WonderWP initializes via its **autoload-wwp** must-use plugin. However, due to a WordPress limitation:

> “WordPress only looks for PHP files directly inside the `mu-plugins` directory and not inside subdirectories.”

To work around this, we provide a **proxy loader file** called:

```
wp-content/mu-plugins/autoload-wwp/wonderwp-mu-proxy.php
```

#### **Move the Proxy File**

Copy the proxy file to the `mu-plugins` directory to ensure WonderWP loads correctly:

```bash
cp wp-content/mu-plugins/autoload-wwp/wonderwp-mu-proxy.php wp-content/mu-plugins/
```

#### **Your `mu-plugins` Folder Should Contain:**

```
wp-content/mu-plugins/
  ├── autoload-wwp/
  ├── generator-wwp/
  ├── wonderwp-mu-proxy.php  # Proxy file now in the correct location
```



***

### 4️⃣ Verify Your Installation

To confirm WonderWP is properly loaded:

1️⃣ **Go to your WordPress Admin Panel**\
2️⃣ **Navigate to:** `Plugins → Must Use`\
3️⃣ **Check if "WonderWP" appears in the list**

📌 If WonderWP is listed, your installation is complete! 🎉

***

### 🎯 Next Steps

✅ **Create Your First Plugin →**\
✅ **Explore WonderWP’s Features →**\
✅ **Join the Community →**

You're now ready to build with WonderWP! 🚀
