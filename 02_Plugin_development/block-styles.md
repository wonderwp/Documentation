# Declaring a Block Style  

WonderWP makes it easy to **declare Block Styles** in a structured and flexible way.  
Inside a WonderWP plugin, all block style declarations should be placed in the **`includes/BlockStyles`** folder.  

As with most WonderWP components, you can declare a Block Style in multiple ways:  
- **[Using the CLI Generator](#using-the-generator)** (Automated)  
- **[Using AI Rules](#using-ai)** (Agentic & Dynamic)  
- **[Class-Based Approach](#class-based)** (Object-Oriented)  
- **[Flat File Approach](#flat-file-based)** (Procedural)  

Each method is designed to **suit different development styles**, so you can choose what fits your workflow best.  

---

## 🚀 Using the CLI Generator  

For **consistency and ease**, WonderWP provides a **CLI command** that generates a fully functional **Block Style class** inside the `includes/BlockStyles` folder.  

✅ **Automatically registered & loaded**  
✅ **Follows best practices**  
✅ **Saves time**  

🔹 **To generate a new Block Style, run:**  

wp wonderwp generate:block-style <block-style-name>

This will create a structured **Block Style class**, already detected and managed by WonderWP.  

---

## 🤖 Using AI Rules  

For **agentic automation**, WonderWP supports **AI-driven Block Style creation**. You can instruct your **agent** to create a Block Style based on predefined **AI rules**.  

### How it works:  
1. The **AI rule follows WonderWP coding standards** by default.  
2. You can **customize the rule** to fit your own conventions.  
3. Simply **ask your AI agent** to generate a Block Style, and it will handle the setup.  

🔹 *Prefer more control? You can write your own AI rules!*  

---

## 🏗 Class-Based Approach  

If you prefer **full control**, you can write your own **Block Style class** and place it inside:  

includes/BlockStyles/

WonderWP will **automatically detect and instantiate** any class inside this folder.  

🔹 *This method is ideal if you want to extend functionality beyond the default implementation.*  

---

## 📄 Flat File Approach  

For developers who prefer **procedural code**, WonderWP allows you to place a **flat PHP file** inside:  

includes/BlockStyles/

This file will be **included and executed automatically** by WonderWP.  

🔹 *This method gives you complete freedom to declare Block Styles the way you normally would in WordPress.*  

---

## 🛠 Choosing the Right Approach  

| Approach             | Best For                          | Pros                                      |  
|----------------------|---------------------------------|-------------------------------------------|  
| **CLI Generator**   | Speed & consistency             | Quick setup, best practices enforced     |  
| **AI Rules**        | Dynamic, agentic workflows      | Automated setup, adaptable to AI agents  |  
| **Class-Based**     | Object-Oriented Development     | Fully customizable, auto-detected        |  
| **Flat File**       | Procedural development          | Maximum flexibility, no OOP required     |  

WonderWP gives you **flexibility and choice**—pick the approach that best suits your workflow! 🚀 