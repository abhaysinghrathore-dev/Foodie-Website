# 🍕 Foodie: Premium Food Delivery Experience

> 🎥 **Video Demonstration**
> [Click here to watch the full project walkthrough](https://drive.google.com/file/d/12FY3YIHaAQ6KReLDRQaF0ildY79naFXG/view?usp=sharing)

---

## 🛡️ Data Privacy & Security Disclosure
> As this application handles sensitive personal information (Resumes/CVs), it is built with a Privacy-First mindset following OWASP AI Security principles. 
> 
> * **Ephemeral Processing**: Uploaded PDF files are processed in-memory. No resume data is permanently stored on the server unless the user explicitly saves it to their profile.
> * **Secure API Orchestration**: All communication with AI Inference Engines (OpenAI/Groq) is encrypted via TLS 1.2+ in transit.
> * **Data Minimization**: We only send the raw text necessary for analysis to the LLM. Personal Identifiable Information (PII) such as Phone Numbers and Addresses can be redacted before processing.
> * **Secrets Management**: API keys and database credentials are never hardcoded. They are managed through Environment Variables and GitHub Secrets for CI/CD pipelines.
> * **Local Deployment Option**: For maximum privacy, users can run the entire stack locally using Docker, ensuring no data ever leaves their machine except for the AI inference call.

*(Note: The above disclosure applies to extended analytics or AI resume integrations attached to this ecosystem.)*

---

## 🗺️ 1. High-Level Overview (The "50,000-Foot" View)

### 🎯 Define the Goal
**Foodie** is an elegant, highly responsive food delivery web application that solves the friction of online food ordering. Its primary purpose is to provide a seamless, tantalizing user experience for hungry customers, enabling them to browse, select, and purchase meals with zero friction. It is designed for:
- **Customers** seeking quick, accessible, and visually appealing menus.
- **Restaurant Owners** wanting a beautiful, conversion-optimized storefront.

### 🍽️ The "Restaurant" Analogy
To understand the architecture without reading code, think of it as a physical restaurant:
- **The Dining Area (Frontend):** (`index.html`, `style.css`) The beautifully decorated environment where the customer sits. It handles the visual presentation and makes the food look irresistible.
- **The Waitstaff (Interactions):** (`ui.js`, `cart.js`) The waitstaff observes your actions, takes your order (*"Add to cart"*), calculates your bill, and ensures a smooth dining experience.
- **The Kitchen & Menu (Data & Logic):** (`products.js`, `products.json`) The kitchen securely stores recipes and ingredients. When the waitstaff asks for the menu, the kitchen rapidly serves it up.

### 📐 System Architecture
```mermaid
graph TD;
    A[Client Browser] -->|Loads UI| B(index.html);
    B -->|Initializes App| C{script.js};
    C -->|Fetches Data| D[(products.json)];
    C -->|Handles Interactions| E[ui.js];
    C -->|Manages State| F[cart.js];
    E -->|Render Menu| G[products.js];
    G -->|Add Item| F;
```

---

## 👥 2. Tailored to the Audience

### 👔 For Non-Technical Stakeholders & Managers
This project is engineered to maximize **User Conversion & Retention**:
- **Zero-Friction Browsing:** A snappy, single-page-like experience means customers find food faster, leading to higher order volume.
- **Business Value:** The modular structure allows instant menu updates (just tweak a JSON file), minimizing developer bottleneck and saving operational costs.
- **Cross-Platform:** Mobile-first design guarantees that customers ordering from their smartphones get the exact premium experience as desktop users.

### 💻 For Developers & Technical Peers
The architecture emphasizes **Modularity & Maintainability**:
- **Data Flow:** Data operates in a strictly unidirectional flow from `products.json` -> `products.js` -> DOM.
- **API Contracts:** Native `fetch()` wrappers are used to decouple static JSON from the frontend, ensuring we can hot-swap `products.json` for an Express/Node.js API endpoint seamlessly in the future.
- **Architectural Decisions ("Why?"):** Vanilla ES6 Modules were chosen over React/Vue to eliminate dependency bloat, maximize initial load speeds (LCP), and maintain tight control over the DOM layout.

---

## 🔍 3. Structured Code Walkthrough

### 🚪 Start at the Entry Point
The application boots up via `script.js`. This is our "Main Controller" that orchestrates the entire application lifecycle:
```javascript
document.addEventListener("DOMContentLoaded", () => {
  productDataFetch();      // Hydrates the DOM with Menu Items
  initUI();                // Binds event listeners
  initCheckout();          // Initializes the shopping cart state
});
```

### 🔗 The "Chain of Actions" (Adding to Cart)
*What happens when a user clicks the "Add to Cart" button?*
1. **Interaction Detect:** An event listener in `products.js` captures the click based on the product's unique `id`.
2. **State Mutation:** The item payload is passed to `cart.js` (`addToCart(product)`), which checks if the item already exists to prevent duplicate entries, and pushes it to the `cartProduct` array.
3. **UI Sync:** The DOM is immediately re-rendered (`updateUI()`) to visually validate the user's action without a page refresh, updating the Cart Badge and Subtotal.

### 🗄️ Highlight Key Data Models
Our core data structure lives in `products.json`:
```json
{
  "id": 1,
  "name": "Double chicken Burger",
  "price": "₹802",
  "image": "images/burger.png"
}
```
This acts as our single source of truth. The application dynamic rendering engine consumes this schema, mapping it to HTML components.

---

## 🎮 4. Interactive and Visual Techniques

### 🐞 Live Debugging & Demo
To see the application pulse in real-time:
1. Open the app in your browser and press `F12` to access Developer Tools.
2. Navigate to the **Sources** tab.
3. Open `cart.js` and set a **Breakpoint** inside the `addToCart` function.
4. Click "Add to Cart" on the website.
5. Watch the `cartProduct` array mutate in real-time as variables shift dynamically.

### ⚙️ Utilize IDE Features
To navigate this codebase efficiently:
- Right-click on methods like `initUI()` and use **"Go to Definition"** (F12 in VS Code) to jump to the logic implementation.
- Use **"Find All References"** on the `products.json` file to instantly see how the frontend parses backend data.

---

## 📦 5. Essential Documentation Components

### 🧠 The "Why" Behind the Code
Instead of simple comments that explain *what* a line does, our codebase uses "Why" comments:
> *"// We use a Map object here instead of a standard Array to guarantee O(1) lookup times when verifying duplicate shopping cart entries, ensuring UI performance at scale."*

### ⏳ Commit History
We treat our Git history as a living journal. Use `git blame [filename]` or `git log --oneline` to view the organic evolution of the files. You will notice clear prefix tags (e.g., `feat:`, `fix:`, `refactor:`) outlining exact progression over time.

---

## 🚀 6. End-to-End Installation Guide

### 🟢 For Non-Technical Users
1. **Download the File:** Click the green `Code` button at the top right of this page and select **Download ZIP**.
2. **Unzip the Folder:** Locate the downloaded file on your computer, right-click, and select "Extract All".
3. **Open the Website:** Open the extracted folder, find the file named `index.html`, and simply double-click it. It will open securely in your default web browser (Chrome, Edge, Safari). You can now play with the site!

### 💻 For Technical Users
To run the full development environment:
1. **Clone the Repo:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/food-delivery-website.git
   ```
2. **Navigate to the Directory:**
   ```bash
   cd food-delivery-website
   ```
3. **Launch the Local Dev Server:**
   * If you are using VS Code, use the [Live Server Extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer). Right-click `index.html` -> **"Open with Live Server"**.
   * Alternatively, use Python/Node:
     ```bash
     npx serve .
     # OR
     python3 -m http.server 8000
     ```
4. **Access the Application:** Open your browser to `http://localhost:8000` (or the port defined by Live Server).

---

## 📄 License
This project is open-sourced under the **MIT License**. You are free to use, modify, distribute, and build upon this code for both personal and commercial use. See the `LICENSE` file for more details.
