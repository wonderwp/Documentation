# Declaring a Custom Post Type

WonderWP makes it easy to **declare Custom Post Types (CPTs)** in a structured and flexible way.\
Inside a WonderWP plugin, all post type declarations should be placed in the **`includes/PostTypes`** folder.

As with most WonderWP components, you can declare a Custom Post Type in multiple ways:

* [**Using the CLI Generator**](custom-post-types.md#using-the-generator) (Automated)
* [**Using AI Rules**](custom-post-types.md#using-ai) (Agentic & Dynamic)
* [**Class-Based Approach**](custom-post-types.md#class-based) (Object-Oriented)
* [**Flat File Approach**](custom-post-types.md#flat-file-based) (Procedural)

Each method is designed to **suit different development styles**, so you can choose what fits your workflow best.

***

## 🚀 Using the CLI Generator

For **consistency and ease**, WonderWP provides a **CLI command** that generates a fully functional **Custom Post Type class** inside the `includes/PostTypes` folder.

✅ **Automatically registered & loaded**\
✅ **Follows best practices**\
✅ **Saves time**

🔹 **To generate a new Custom Post Type, run:**

```bash
wp wonderwp generate:cpt <post-type-name>
```

This will create a structured **CustomPostType class**, already detected and managed by WonderWP.

***

### 🤖 Using AI Rules

For **agentic automation**, WonderWP supports **AI-driven CPT creation**. You can instruct your **agent** to create a Custom Post Type based on predefined **AI rules**.

#### How it works:

1. The **AI rule follows WonderWP coding standards** by default.
2. You can **customize the rule** to fit your own conventions.
3. Simply **ask your AI agent** to generate a Custom Post Type, and it will handle the setup.

🔹 _Prefer more control? You can write your own AI rules!_

***

### 🏗 Class-Based Approach

If you prefer **full control**, you can write your own **Custom Post Type class** and place it inside:

```
includes/PostTypes/
```

WonderWP will **automatically detect and instantiate** any class inside this folder.

🔹 _This method is ideal if you want to extend functionality beyond the default implementation._

***

### 📄 Flat File Approach

For developers who prefer **procedural code**, WonderWP allows you to place a **flat PHP file** inside:

```
swiftCopierModifierincludes/PostTypes/
```

This file will be **included and executed automatically** by WonderWP.

🔹 _This method gives you complete freedom to declare CPTs the way you normally would in WordPress._

***

### 🛠 Choosing the Right Approach

| Approach          | Best For                    | Pros                                    |
| ----------------- | --------------------------- | --------------------------------------- |
| **CLI Generator** | Speed & consistency         | Quick setup, best practices enforced    |
| **AI Rules**      | Dynamic, agentic workflows  | Automated setup, adaptable to AI agents |
| **Class-Based**   | Object-Oriented Development | Fully customizable, auto-detected       |
| **Flat File**     | Procedural development      | Maximum flexibility, no OOP required    |

WonderWP gives you **flexibility and choice**—pick the approach that best suits your workflow! 🚀
