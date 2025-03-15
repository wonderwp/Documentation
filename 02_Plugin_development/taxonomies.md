# Declaring a Taxonomy

WonderWP makes it easy to **declare Taxonomies** in a structured and flexible way.\
Inside a WonderWP plugin, all taxonomy declarations should be placed in the **`includes/Taxonomies`** folder.

As with most WonderWP components, you can declare a Taxonomy in multiple ways:

* [**Using the CLI Generator**](taxonomies.md#using-the-generator) (Automated)
* [**Using AI Rules**](taxonomies.md#using-ai) (Agentic & Dynamic)
* [**Class-Based Approach**](taxonomies.md#class-based) (Object-Oriented)
* [**Flat File Approach**](taxonomies.md#flat-file-based) (Procedural)

Each method is designed to **suit different development styles**, so you can choose what fits your workflow best.

***

## 🚀 Using the CLI Generator

For **consistency and ease**, WonderWP provides a **CLI command** that generates a fully functional **Taxonomy class** inside the `includes/Taxonomies` folder.

✅ **Automatically registered & loaded**\
✅ **Follows best practices**\
✅ **Saves time**

🔹 **To generate a new Taxonomy, run:**

```bash
wp wonderwp generate:taxonomy
```



This will create a structured **Taxonomy class**, already detected and managed by WonderWP.

***

## 🤖 Using AI Rules

For **agentic automation**, WonderWP supports **AI-driven Taxonomy creation**. You can instruct your **agent** to create a Taxonomy based on predefined **AI rules**.

### How it works:

1. The **AI rule follows WonderWP coding standards** by default.
2. You can **customize the rule** to fit your own conventions.
3. Simply **ask your AI agent** to generate a Taxonomy, and it will handle the setup.

🔹 _Prefer more control? You can write your own AI rules!_

***

## 🏗 Class-Based Approach

If you prefer **full control**, you can write your own **Taxonomy class** and place it inside:

includes/Taxonomies/

WonderWP will **automatically detect and instantiate** any class inside this folder.

🔹 _This method is ideal if you want to extend functionality beyond the default implementation._

***

## 📄 Flat File Approach

For developers who prefer **procedural code**, WonderWP allows you to place a **flat PHP file** inside:

includes/Taxonomies/

This file will be **included and executed automatically** by WonderWP.

🔹 _This method gives you complete freedom to declare Taxonomies the way you normally would in WordPress._

***

## 🛠 Choosing the Right Approach

| Approach          | Best For                    | Pros                                    |
| ----------------- | --------------------------- | --------------------------------------- |
| **CLI Generator** | Speed & consistency         | Quick setup, best practices enforced    |
| **AI Rules**      | Dynamic, agentic workflows  | Automated setup, adaptable to AI agents |
| **Class-Based**   | Object-Oriented Development | Fully customizable, auto-detected       |
| **Flat File**     | Procedural development      | Maximum flexibility, no OOP required    |

WonderWP gives you **flexibility and choice**—pick the approach that best suits your workflow! 🚀
