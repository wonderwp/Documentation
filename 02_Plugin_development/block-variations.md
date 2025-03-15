# Declaring a Block Variation

WonderWP makes it easy to **declare Block Variations** in a structured and flexible way.\
Inside a WonderWP plugin, all block variation declarations should be placed in the **`includes/BlockVariations`** folder.

As with most WonderWP components, you can declare a Block Variation in multiple ways:

* [**Using the CLI Generator**](block-variations.md#using-the-generator) (Automated)
* [**Using AI Rules**](block-variations.md#using-ai) (Agentic & Dynamic)
* [**Class-Based Approach**](block-variations.md#class-based) (Object-Oriented)
* [**Flat File Approach**](block-variations.md#flat-file-based) (Procedural)

Each method is designed to **suit different development styles**, so you can choose what fits your workflow best.

***

## 🚀 Using the CLI Generator

For **consistency and ease**, WonderWP provides a **CLI command** that generates a fully functional **Block Variation class** inside the `includes/BlockVariations` folder.

✅ **Automatically registered & loaded**\
✅ **Follows best practices**\
✅ **Saves time**

🔹 **To generate a new Block Variation, run:**

```bash
wp wonderwp generate block-variation
```

This will create a structured **Block Variation class**, already detected and managed by WonderWP.

***

## 🤖 Using AI Rules

For **agentic automation**, WonderWP supports **AI-driven Block Variation creation**. You can instruct your **agent** to create a Block Variation based on predefined **AI rules**.

### How it works:

1. The **AI rule follows WonderWP coding standards** by default.
2. You can **customize the rule** to fit your own conventions.
3. Simply **ask your AI agent** to generate a Block Variation, and it will handle the setup.

🔹 _Prefer more control? You can write your own AI rules!_

***

## 🏗 Class-Based Approach

If you prefer **full control**, you can write your own **Block Variation class** and place it inside:

includes/BlockVariations/

WonderWP will **automatically detect and instantiate** any class inside this folder.

🔹 _This method is ideal if you want to extend functionality beyond the default implementation._

***

## 📄 Flat File Approach

For developers who prefer **procedural code**, WonderWP allows you to place a **flat PHP file** inside:

includes/BlockVariations/

This file will be **included and executed automatically** by WonderWP.

🔹 _This method gives you complete freedom to declare Block Variations the way you normally would in WordPress._

***

## 🛠 Choosing the Right Approach

| Approach          | Best For                    | Pros                                    |
| ----------------- | --------------------------- | --------------------------------------- |
| **CLI Generator** | Speed & consistency         | Quick setup, best practices enforced    |
| **AI Rules**      | Dynamic, agentic workflows  | Automated setup, adaptable to AI agents |
| **Class-Based**   | Object-Oriented Development | Fully customizable, auto-detected       |
| **Flat File**     | Procedural development      | Maximum flexibility, no OOP required    |

WonderWP gives you **flexibility and choice**—pick the approach that best suits your workflow! 🚀
