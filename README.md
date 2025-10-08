### **Assignment: “Shopify Product Summary Microservice”**

#### **Scenario**

You’ve joined our team to help integrate our analytics platform with Shopify. Your task is to build a lightweight Node.js microservice that exposes a RESTful API for retrieving product data, along with a TypeScript SDK that wraps each endpoint for easy reuse across our platform.

---

### **Requirements**

#### **1\. Setup**

* Create a project in your personal Github repository that you can share.  You will email the link to the completed project to [cpetro@luckyorange.com](mailto:cpetro@luckyorange.com) when you are ready.

* Inside this github project, you will find a [main.db.zip](https://github.com/locpetro/luckyorange-integrations-technical-assessment/blob/main/README.md) file.  Unzip this file using the password provided in the technical assessment email we sent.

* In your code, connect to the **`main.db`** file to query for the client credentials and use the store/access token for `id=1` when performing your shopify queries.  Here is what the table schema looks like:

  ```sql
  CREATE TABLE IF NOT EXISTS shopify_credentials(
    id INTEGER PRIMARY KEY,
    store text not null,
    access_token text not null
  ) STRICT
  ```
  *Note: do not check in the main.db into your project*

#### **2\. Microservice**

Build a Node.js REST API (Express.js or Hono preferred) with the following endpoints:

##### **`GET /products`**

Fetch a list of products (sorted by title) from the connected Shopify store and return a simplified JSON response with the following structure:

```json
[
  {  
    "id": "gid://shopify/Product/1234567890",
    "title": "Example Product",
    "price": 49.99,
    "inventory": 120,
    "created_at": "2025-01-01T12:00:00Z"
  }
]
```

##### **`GET /products/:id`**

Return the detailed information of a single product by ID.

**`GET /stats`**

Return an aggregated summary of the store’s products, e.g.:

```json
{
  "total_products": 24,
  "total_inventory": 548,
  "average_price": 36.74
}
```

---

### **3\. Technical Expectations**

* Use environment variables for credentials.

* Use `axios` or `node-fetch` for HTTP requests.

* Use the **Shopify GraphQL Admin API** instead of REST.

* Provide SDK with the following methods:

  * **`getProducts(): Product[]`**

  * **`getProductById(id: string): Product`**

  * **`getStats(): ProductStats`**

---

### **4\. Bonus Points**

* Implement a caching layer (in-memory or Redis) for performance.

* Use TypeScript for type safety.

---

### **Submission**

* Check your code into your personal github account

* Email the link to the completed project to [cpetro@luckyorange.com](mailto:cpetro@luckyorange.com).
