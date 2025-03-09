# Declaring a Custom Post Type  

WonderWP makes it easy to **declare Custom Post Types (CPTs)** in a structured and flexible way.  
Inside a WonderWP plugin, all post type declarations should be placed in the **`includes/PostTypes`** folder.  

As with most WonderWP components, you can declare a Custom Post Type in multiple ways:  
- **[Using the CLI Generator](#using-the-generator)** (Automated)  
- **[Using AI Rules](#using-ai)** (Agentic & Dynamic)  
- **[Class-Based Approach](#class-based)** (Object-Oriented)  
- **[Flat File Approach](#flat-file-based)** (Procedural)  

Each method is designed to **suit different development styles**, so you can choose what fits your workflow best.  

---

## 🚀 Using the CLI Generator  

For **consistency and ease**, WonderWP provides a **CLI command** that generates a fully functional **Custom Post Type class** inside the `includes/PostTypes` folder.  

✅ **Automatically registered & loaded**  
✅ **Follows best practices**  
✅ **Saves time**  

🔹 **To generate a new Custom Post Type, run:**  

```bash
wp wonderwp generate:cpt <post-type-name>
