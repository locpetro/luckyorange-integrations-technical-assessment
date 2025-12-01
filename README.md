## **Assignment: “Shopify Product Summary Microservice”**

### **Scenario**

You are building a high-performance "Product Summary Service" for Lucky Orange. This service will be used by our dashboard to display quick product snapshots to merchants.

#### **The Problem:**

- We have thousands of concurrent users.
- The Dashboard SLA requires API responses in under 50ms.
- The upstream Shopify Admin GraphQL API averages 300ms–500ms per request and imposes strict rate limits.

**Your Task:** Build a Node.js microservice (along with a SDK that our customers consume) that acts as a middleware between our customers and Shopify.

#### **Key Architectural Requirements:**

1. **Latency:** You must implement a strategy (e.g., Caching) to meet the <50ms response time. Direct proxying to Shopify will not pass.

2. **Scalability:** The service will run on multiple instances (containers). Your caching or state strategy must account for this (avoid stateful local memory that desyncs across instances).

3. **Resilience:** The SDK must handle Shopify's GraphQL rate limits (Leaky Bucket) robustly.

4. **Data:** Use the provided credentials file to make Shopify GrpahQL requests.

---

#### **Requirements**

##### **1\. Setup**

* Create a project in your personal Github repository that you can share.  You will email the link to the completed project to [cpetro@luckyorange.com](mailto:cpetro@luckyorange.com) when you are ready.

* Inside this github project, you will find a [shopify_credentials.zip](https://github.com/locpetro/luckyorange-integrations-technical-assessment/blob/main/shopify_credentials.zip) file.  Unzip this file using the password provided in the technical assessment email we sent.

* In your code, expose the Shopify shop + token in your app via environment variables.

#### **2\. Microservice**

Build a Node.js REST API (Express.js or Hono preferred) with the following endpoints:

#### **`GET /products?limit=x&cursor=xxx`**

Fetch a list of products (sorted by title) using optional Cursor-based pagination (expose a next_page token in your API response) from the connected Shopify store and return a JSON response with the following product data:

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

#### **`GET /products/:id`**

Return the detailed information of a single product by ID.

```json
{  
  "id": "gid://shopify/Product/1234567890",
  "title": "Example Product",
  "price": 49.99,
  "inventory": 120,
  "created_at": "2025-01-01T12:00:00Z"
}
```

#### **`GET /api-stats`**

Return an aggregated latency summary, e.g.:

```json
{
  "endpoint_response_times_ms": {
    "average": 10,
    "max": 15,
    "min": 8
  },
  "total_endpoint_calls": 20,
  "average_shopify_call_responsetime_ms": 200
  "total_shopify_api_calls": 5
}
```

---

### **3\. Technical Expectations**

* Use `axios` or `node-fetch` for HTTP requests.

* Use the **Shopify GraphQL Admin API** instead of REST.

* Provide SDK with the following methods:

  * **`getProducts(limit?: number, cursor?: string): Promise<ProductResponse>`**

  * **`getProductById(id: string): Promise<Product>`**

  * **`getStats(): Promise<ProductStats>`**

---

### **Submission**

* Check your code into your personal github account

* Email the link to the completed project to [cpetro@luckyorange.com](mailto:cpetro@luckyorange.com).
