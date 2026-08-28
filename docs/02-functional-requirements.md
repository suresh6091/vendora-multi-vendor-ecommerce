# Vendora — Functional Requirements

**Document:** Functional Requirements
**Version:** 1.0
**Status:** Initial Architecture Draft
**Project:** Vendora
**Repository:** `vendora-multi-vendor-ecommerce`

---

## 1. Purpose

This document defines the functional requirements for **Vendora**, a multi-vendor e-commerce marketplace.

The purpose of this document is to describe **what the system must do** from a business and user perspective.

This document does not define implementation details such as database tables, API endpoints, programming languages, infrastructure, or deployment configuration. Those details will be defined in separate architecture and technical design documents.

---

# 2. System Actors

Vendora will have the following primary actors:

```text
                    Vendora
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
      Customer       Seller       Admin
```

### 2.1 Customer

A customer uses Vendora to discover products and purchase products from sellers.

### 2.2 Seller

A seller uses Vendora to list products and manage inventory, orders, and sales.

### 2.3 Administrator

An administrator manages and controls the overall marketplace.

### 2.4 AI Assistants

AI assistants provide conversational support and product discovery capabilities.

They operate as supporting components and must follow the same authorization and business rules as other application clients.

---

# 3. Customer Functional Requirements

## FR-CUS-001: Customer Registration

The system shall allow a new customer to create an account.

The registration process shall collect the required account information and validate the submitted information.

The system shall prevent duplicate accounts according to the platform's identity rules.

---

## FR-CUS-002: Customer Authentication

The system shall allow registered customers to securely log in.

The system shall authenticate the customer's credentials before granting access to protected resources.

The system shall allow customers to log out.

---

## FR-CUS-003: Customer Profile

The system shall allow customers to:

* View their profile.
* Update their profile information.
* Manage account information.
* Change their password.
* Manage saved addresses.

---

## FR-CUS-004: Address Management

Customers shall be able to:

* Add a delivery address.
* View saved addresses.
* Edit an address.
* Delete an address.
* Select a default delivery address.

An address used by an existing order shall remain associated with that order even if the customer later changes their saved address.

---

# 4. Product Discovery Requirements

## FR-PRO-001: Product Browsing

Customers shall be able to browse available products.

The product catalog shall provide information including, where applicable:

* Product name.
* Description.
* Price.
* Images.
* Category.
* Seller.
* Rating.
* Availability.

---

## FR-PRO-002: Product Details

Customers shall be able to view detailed information about an individual product.

The product details page shall display relevant product information and purchasing options.

---

## FR-PRO-003: Product Search

The system shall allow customers to search for products using keywords.

Search results shall return products relevant to the customer's search query.

---

## FR-PRO-004: Product Filtering

Customers shall be able to filter products using supported attributes such as:

* Category.
* Price range.
* Seller.
* Rating.
* Availability.
* Other product-specific attributes.

---

## FR-PRO-005: Product Sorting

Customers shall be able to sort products using supported options such as:

* Price: low to high.
* Price: high to low.
* Rating.
* Newest products.
* Popularity.

---

## FR-PRO-006: Product Availability

The system shall display whether a product is available for purchase.

Products that cannot be purchased due to insufficient inventory shall not be allowed to proceed through checkout.

---

# 5. Category Requirements

## FR-CAT-001: Category Browsing

Customers shall be able to browse products by category.

---

## FR-CAT-002: Category Hierarchy

The system shall support categories and subcategories.

Example:

```text
Electronics
├── Mobile Phones
├── Laptops
├── Tablets
└── Accessories
```

---

## FR-CAT-003: Category Management

Administrators shall be able to:

* Create categories.
* Update categories.
* Remove categories where permitted.
* Manage category hierarchy.

---

# 6. Seller Functional Requirements

## FR-SEL-001: Seller Registration

The system shall allow users to apply to become sellers.

The seller registration process shall collect the required seller information.

---

## FR-SEL-002: Seller Approval

The system shall support seller approval by administrators when seller approval is required.

A seller shall not receive seller privileges until the required approval process has been completed.

---

## FR-SEL-003: Seller Profile

Sellers shall be able to:

* View their seller profile.
* Update permitted seller information.
* Manage their marketplace presence.

---

## FR-SEL-004: Seller Product Creation

Authorized sellers shall be able to create product listings.

A product listing may contain:

* Product name.
* Description.
* Category.
* Price.
* Images.
* Product attributes.
* Inventory information.

---

## FR-SEL-005: Seller Product Management

Sellers shall be able to manage products they own.

They shall be able to:

* View their products.
* Update products.
* Update prices.
* Update product information.
* Manage product availability.
* Remove or deactivate products where permitted.

A seller shall not be able to modify another seller's products.

---

## FR-SEL-006: Seller Inventory Management

Sellers shall be able to:

* View inventory.
* Update stock quantities.
* View available stock.
* View reserved stock where applicable.

Inventory changes shall follow the platform's inventory rules.

---

## FR-SEL-007: Seller Order Management

Sellers shall be able to view orders containing their products.

Sellers shall be able to process eligible orders according to the defined order lifecycle.

---

## FR-SEL-008: Seller Sales Information

Sellers shall be able to view relevant information about their sales and orders.

Detailed reporting requirements will be defined in a future reporting specification.

---

# 7. Cart Requirements

## FR-CART-001: Add Product to Cart

Customers shall be able to add an available product to their cart.

---

## FR-CART-002: View Cart

Customers shall be able to view:

* Products in the cart.
* Quantities.
* Product prices.
* Item totals.
* Applicable discounts.
* Estimated order total.

---

## FR-CART-003: Update Cart Quantity

Customers shall be able to increase or decrease the quantity of cart items subject to inventory availability.

---

## FR-CART-004: Remove Cart Item

Customers shall be able to remove products from their cart.

---

## FR-CART-005: Cart Validation

The system shall validate cart items before checkout.

Validation shall include, where applicable:

* Product availability.
* Current price.
* Inventory availability.
* Seller availability.
* Coupon validity.

The system shall not assume that cart information remains valid indefinitely.

---

# 8. Wishlist Requirements

## FR-WIS-001: Add to Wishlist

Customers shall be able to add products to their wishlist.

---

## FR-WIS-002: Remove from Wishlist

Customers shall be able to remove products from their wishlist.

---

## FR-WIS-003: View Wishlist

Customers shall be able to view their saved products.

---

# 9. Coupon Requirements

## FR-COU-001: Coupon Creation

Authorized administrators shall be able to create coupons.

A coupon may contain rules such as:

* Coupon code.
* Discount type.
* Discount value.
* Minimum order value.
* Maximum discount.
* Start date.
* Expiration date.
* Usage limits.
* Eligibility restrictions.

---

## FR-COU-002: Coupon Validation

The system shall validate a coupon before applying its discount.

Invalid, expired, inactive, or ineligible coupons shall not be applied.

---

## FR-COU-003: Coupon Application

Customers shall be able to apply an eligible coupon during the shopping or checkout process.

The system shall calculate the discount according to the coupon rules.

---

# 10. Checkout Requirements

## FR-CHK-001: Checkout Initiation

Customers shall be able to proceed to checkout when their cart contains valid purchasable items.

---

## FR-CHK-002: Checkout Information

The checkout process shall allow customers to confirm:

* Products.
* Quantities.
* Delivery address.
* Applicable coupon.
* Order total.
* Payment method.

---

## FR-CHK-003: Final Order Validation

Before creating an order, the system shall validate the latest:

* Product information.
* Product price.
* Inventory.
* Coupon eligibility.
* Delivery information.

The system shall prevent an order from being created when required validation fails.

---

# 11. Order Requirements

## FR-ORD-001: Order Creation

The system shall allow a customer to create an order from a valid checkout.

An order shall contain the products and quantities purchased by the customer.

---

## FR-ORD-002: Order Identification

Each order shall have a unique order identifier.

---

## FR-ORD-003: Order Items

The system shall maintain individual order items for products purchased in an order.

Order item information shall preserve the relevant purchase information at the time the order is created.

---

## FR-ORD-004: Order History

Customers shall be able to view their previous orders.

---

## FR-ORD-005: Order Details

Customers shall be able to view details of an individual order, including:

* Order identifier.
* Products.
* Quantities.
* Prices.
* Order total.
* Payment status.
* Shipping status.
* Order status.

---

## FR-ORD-006: Order Status

The system shall maintain an order lifecycle.

The initial lifecycle may include:

```text
PENDING
    ↓
CONFIRMED
    ↓
PROCESSING
    ↓
SHIPPED
    ↓
DELIVERED
```

Alternative states such as cancellation, payment failure, return, or refund shall be defined by the order lifecycle specification.

---

## FR-ORD-007: Order Cancellation

Customers shall be able to request cancellation of eligible orders.

The system shall determine whether cancellation is permitted based on the current order state and business rules.

---

## FR-ORD-008: Seller Order Processing

Sellers shall be able to process orders containing their products.

Seller actions shall be restricted according to the order lifecycle.

---

## FR-ORD-009: Administrator Order Management

Administrators shall be able to view and manage marketplace orders according to their permissions.

---

# 12. Inventory Requirements

## FR-INV-001: Product Stock

The system shall maintain inventory information for products.

---

## FR-INV-002: Stock Availability

The system shall determine whether sufficient inventory exists before allowing a purchase.

---

## FR-INV-003: Inventory Reservation

The system shall support inventory reservation during the order process where required.

Reserved inventory shall not be incorrectly made available to another order.

---

## FR-INV-004: Inventory Update

The system shall update inventory according to defined order and fulfillment events.

---

## FR-INV-005: Inventory Release

Inventory reserved for an unsuccessful or cancelled transaction shall be released according to the applicable business rules.

---

# 13. Payment Requirements

## FR-PAY-001: Payment Initiation

The system shall initiate payment processing for eligible orders.

---

## FR-PAY-002: Payment Status

The system shall maintain payment status information.

Example states may include:

```text
PENDING
SUCCESS
FAILED
CANCELLED
REFUNDED
```

---

## FR-PAY-003: Dummy Payment Gateway

The initial version shall provide a dummy payment gateway for development and testing.

The payment functionality shall be designed behind an abstraction so that a real payment provider can be integrated later.

---

## FR-PAY-004: Payment and Order Consistency

The system shall ensure that payment results are correctly associated with the corresponding order.

Payment failures shall not incorrectly mark an order as successfully paid.

---

# 14. Shipping Requirements

## FR-SHP-001: Shipping Information

The system shall maintain shipping information associated with an order.

---

## FR-SHP-002: Shipment Creation

A shipment shall be created for an eligible order according to the defined order fulfillment process.

---

## FR-SHP-003: Shipment Status

The system shall maintain shipment states.

Example:

```text
PENDING
    ↓
PACKED
    ↓
SHIPPED
    ↓
OUT_FOR_DELIVERY
    ↓
DELIVERED
```

---

## FR-SHP-004: Shipment Tracking

Customers shall be able to view available shipment tracking information.

---

# 15. Review and Rating Requirements

## FR-REV-001: Product Review

Customers shall be able to submit reviews for eligible purchased products.

---

## FR-REV-002: Product Rating

Customers shall be able to provide a rating for eligible products.

---

## FR-REV-003: Review Ownership

A customer shall only be able to modify or remove reviews that they own, subject to platform rules.

---

## FR-REV-004: Review Moderation

Administrators shall be able to review and moderate reviews according to marketplace policies.

---

# 16. Notification Requirements

## FR-NOT-001: Order Notifications

The system shall support notifications for important order events.

Examples:

* Order created.
* Order confirmed.
* Order cancelled.
* Order shipped.
* Order delivered.

---

## FR-NOT-002: Payment Notifications

The system shall support notifications for relevant payment events.

---

## FR-NOT-003: Shipping Notifications

The system shall support notifications for relevant shipment events.

---

## FR-NOT-004: Notification Delivery

The notification system shall be designed so additional delivery channels can be introduced later.

Potential channels include:

* Email.
* In-app notifications.
* Push notifications.

---

# 17. Administrator Requirements

## FR-ADM-001: User Management

Administrators shall be able to:

* View users.
* Manage user status.
* Manage appropriate user roles.
* Perform permitted administrative actions.

---

## FR-ADM-002: Seller Management

Administrators shall be able to:

* View sellers.
* Review seller applications.
* Approve eligible sellers.
* Reject eligible applications.
* Suspend sellers when permitted.

---

## FR-ADM-003: Product Management

Administrators shall be able to manage marketplace products according to administrative permissions.

---

## FR-ADM-004: Category Management

Administrators shall be able to manage categories and subcategories.

---

## FR-ADM-005: Order Management

Administrators shall be able to view marketplace orders and perform permitted administrative actions.

---

## FR-ADM-006: Coupon Management

Administrators shall be able to create, update, deactivate, and manage coupons.

---

## FR-ADM-007: Review Management

Administrators shall be able to moderate reviews according to platform policies.

---

# 18. Authentication and Authorization Requirements

## FR-AUTH-001: Role-Based Access

The system shall support role-based access control.

Initial roles:

```text
CUSTOMER
SELLER
ADMIN
```

---

## FR-AUTH-002: Protected Resources

Protected operations shall require appropriate authentication.

---

## FR-AUTH-003: Authorization

The system shall verify that an authenticated user has permission to perform a requested operation.

For example:

```text
Customer
    ↓
Can manage own cart
Can place own orders
Can manage own wishlist

Seller
    ↓
Can manage own products
Can manage own inventory
Can process eligible seller orders

Admin
    ↓
Can manage marketplace resources
```

---

## FR-AUTH-004: Resource Ownership

Users shall not be able to access or modify resources belonging to another user unless explicitly authorized.

---

# 19. AI Support Assistant Requirements

## FR-AI-001: Customer Support

The Support Assistant shall allow customers to ask questions using natural language.

Example:

```text
"Where is my order?"
"Has my order been shipped?"
"How can I cancel my order?"
```

---

## FR-AI-002: Order Information

The Support Assistant may provide order-related information when the authenticated customer has permission to access that information.

---

## FR-AI-003: Business Rule Enforcement

The AI assistant shall not bypass application authorization or business rules.

For example, the AI shall not directly modify an order without going through an authorized application operation.

---

## FR-AI-004: AI Provider Abstraction

The AI functionality shall be designed so that the application is not tightly coupled to a single AI provider.

---

# 20. AI Product Assistant Requirements

## FR-AIP-001: Natural Language Product Discovery

The Product Assistant shall allow customers to describe what they are looking for using natural language.

Example:

```text
"I need a laptop for programming
under ₹70,000 with at least 16GB RAM."
```

---

## FR-AIP-002: Product Search

The Product Assistant shall use available product information to help identify relevant products.

---

## FR-AIP-003: Product Comparison

The Product Assistant may help customers compare products based on relevant attributes.

---

## FR-AIP-004: Product Recommendations

The Product Assistant may recommend products based on customer requirements and available catalog information.

Recommendations shall be based on available product data and shall not invent product specifications.

---

## FR-AIP-005: Authorized Actions

Future versions may allow the Product Assistant to perform actions such as:

```text
"Add this product to my cart."
```

Such actions shall be executed through authorized application services rather than direct database access.

---

# 21. Marketplace Requirements

## FR-MKT-001: Multiple Sellers

The system shall support multiple independent sellers on the same marketplace.

---

## FR-MKT-002: Seller Product Ownership

Each seller shall only be able to manage products belonging to that seller.

---

## FR-MKT-003: Seller Identification

Customers shall be able to identify the seller associated with a product.

---

## FR-MKT-004: Multi-Seller Orders

The system shall support the possibility of a customer purchasing products from multiple sellers within a single shopping session.

The exact order splitting and fulfillment behavior will be defined in the order architecture.

---

## FR-MKT-005: Seller Isolation

Seller-specific information and operations shall be isolated so that one seller cannot access another seller's protected business information.

---

# 22. Search and Discovery Requirements

The product discovery system shall support:

* Keyword search.
* Category browsing.
* Filtering.
* Sorting.
* Product availability.
* Seller information.

Future versions may introduce:

* Natural-language search.
* Semantic search.
* Personalized recommendations.
* AI-powered discovery.

---

# 23. Business Rules

The following high-level business rules shall apply:

### BR-001: Product Ownership

A seller can manage only products owned by that seller.

### BR-002: Inventory

A product cannot be purchased when sufficient inventory is unavailable.

### BR-003: Order Price

The final order shall use the validated purchase price at the time the order is created.

### BR-004: Coupon

A coupon shall only be applied when all coupon eligibility rules are satisfied.

### BR-005: Order Cancellation

Order cancellation shall depend on the current order state.

### BR-006: Reviews

Only eligible customers shall be allowed to submit reviews according to platform rules.

### BR-007: Authorization

Every protected operation shall verify user permissions.

### BR-008: AI Access

AI assistants shall not bypass normal application authorization or business rules.

---

# 24. Primary Customer Journey

The primary customer journey is:

```text
Register / Login
       ↓
Browse / Search Products
       ↓
View Product
       ↓
Add to Cart
       ↓
Review Cart
       ↓
Apply Coupon
       ↓
Checkout
       ↓
Select Address
       ↓
Payment
       ↓
Order Created
       ↓
Order Processing
       ↓
Shipment
       ↓
Delivery
       ↓
Review Product
```

---

# 25. Primary Seller Journey

The primary seller journey is:

```text
Seller Registration
       ↓
Seller Approval
       ↓
Seller Profile
       ↓
Create Product
       ↓
Manage Inventory
       ↓
Receive Order
       ↓
Process Order
       ↓
Prepare Shipment
       ↓
Order Shipped
       ↓
Order Delivered
       ↓
View Sales Information
```

---

# 26. Primary Administrator Journey

The primary administrator journey is:

```text
Admin Login
     ↓
Admin Dashboard
     ↓
Manage Users
     ↓
Manage Sellers
     ↓
Manage Categories
     ↓
Manage Products
     ↓
Manage Orders
     ↓
Manage Coupons
     ↓
Moderate Reviews
     ↓
Monitor Marketplace
```

---

# 27. Functional Scope Summary

The first major version of Vendora shall include:

```text
Customer
├── Registration / Login
├── Profile
├── Addresses
├── Product Discovery
├── Search
├── Filtering / Sorting
├── Cart
├── Wishlist
├── Coupons
├── Checkout
├── Orders
├── Payments
├── Shipping
└── Reviews

Seller
├── Registration
├── Approval
├── Profile
├── Product Management
├── Inventory Management
├── Order Management
└── Sales Information

Admin
├── User Management
├── Seller Management
├── Product Management
├── Category Management
├── Order Management
├── Coupon Management
└── Review Management

AI
├── Support Assistant
└── Product Assistant
```

---

# 28. Future Functional Capabilities

The following capabilities may be considered after the initial system is stable:

* Advanced product recommendations.
* Personalized home pages.
* Natural-language product search.
* AI-powered product comparison.
* AI-assisted customer support.
* Return and refund management.
* Seller analytics.
* Advanced marketplace analytics.
* Loyalty programs.
* Promotional campaigns.
* Multiple payment providers.
* Multiple shipping providers.
* Internationalization.
* Multi-currency support.

These capabilities are not required for the initial implementation unless explicitly added to the project scope.

---

# 29. Requirement Traceability

Each functional requirement uses a unique identifier.

Examples:

```text
FR-CUS-001  Customer Registration
FR-SEL-001  Seller Registration
FR-PRO-001  Product Browsing
FR-CART-001 Cart Management
FR-ORD-001  Order Creation
FR-PAY-001  Payment Processing
FR-INV-001  Inventory Management
FR-SHP-001  Shipping
FR-REV-001  Reviews
FR-AI-001   Support Assistant
```

These identifiers can be referenced later in:

* API specifications.
* Database design.
* Test cases.
* Implementation tasks.
* Pull requests.
* Issue tracking.

---

# 30. Document Status

**Document:** Functional Requirements
**Version:** 1.0
**Status:** Initial Architecture Draft
**Project:** Vendora
**Repository:** `vendora-multi-vendor-ecommerce`

This document establishes the functional scope for the initial Vendora platform. Detailed technical architecture, database design, API specifications, security design, and implementation decisions will be documented separately.
