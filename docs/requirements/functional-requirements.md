# Functional Requirements - Music Band T-Shirt E-Commerce Store

## Overview

This document defines the functional requirements (FRs) for the Music Band T-Shirt E-Commerce Store, specifying what the system must do from a user and business perspective.

---

## FR1: User Authentication & Account Management

### FR1.1: User Registration

- **Requirement**: The system shall allow new users to create an account using email and password
- **Input**: Email address, password (min 8 characters with 1 uppercase, 1 number), password confirmation
- **Output**: User account created, verification email sent
- **Validation**:
  - Email format validation (RFC 5322 compliant)
  - Password strength requirements enforced
  - Duplicate email addresses rejected
  - CSRF token validation
- **Business Rules**:
  - Email verification required before full account access
  - Verification link expires after 24 hours
  - Rate limit: 5 registration attempts per IP per hour

### FR1.2: User Login

- **Requirement**: The system shall authenticate registered users with email and password
- **Input**: Email address, password, optional "remember me" flag
- **Output**: JWT access token (1 hour), refresh token (7 days or 30 days with "remember me")
- **Validation**:
  - Email and password must match registered credentials
  - Account must be verified
  - Rate limiting on failed attempts
- **Business Rules**:
  - Max 5 failed login attempts per account per 15 minutes
  - Account locked after 10 failed attempts (requires password reset)
  - Tokens stored in httpOnly cookies

### FR1.3: Password Reset

- **Requirement**: The system shall allow users to reset forgotten passwords via email
- **Input**: Registered email address
- **Output**: Password reset link sent via email
- **Validation**:
  - Email must exist in system
  - Reset token unique and secure
- **Business Rules**:
  - Reset link expires after 1 hour
  - Token can only be used once
  - All existing sessions invalidated on password change
  - Rate limit: 3 reset requests per email per hour

### FR1.4: Profile Management

- **Requirement**: The system shall allow users to view and update their profile information
- **Input**: Full name, phone number, default shipping address
- **Output**: Updated profile data
- **Validation**:
  - Phone number format validation
  - Address completeness validation
- **Business Rules**:
  - Email changes require verification of new email
  - Profile changes logged for audit purposes

### FR1.5: User Logout

- **Requirement**: The system shall allow users to securely log out
- **Input**: Logout request
- **Output**: Session terminated, tokens invalidated
- **Business Rules**:
  - Refresh token blacklisted until expiration
  - Cart data persists for logged-in users

---

## FR2: Product Catalog Management

### FR2.1: Product Listing

- **Requirement**: The system shall display a paginated list of available products
- **Input**: Page number, items per page (default 24), optional filters
- **Output**: Product cards with image, title, band name, price, availability
- **Validation**:
  - Page number must be positive integer
  - Items per page between 1 and 100
- **Business Rules**:
  - Only active products displayed to customers
  - Out-of-stock products marked clearly
  - Products sorted by: newest, price (low-high, high-low), popularity

### FR2.2: Product Details

- **Requirement**: The system shall display detailed information for a specific product
- **Input**: Product ID
- **Output**: Product name, band, description, images, available variants (size, color), price, stock status
- **Validation**:
  - Product ID must exist
  - Product must be active
- **Business Rules**:
  - Stock levels checked in real-time
  - Multiple images displayed in gallery
  - Related products suggested (4-6 items)

### FR2.3: Product Search

- **Requirement**: The system shall allow users to search for products by keyword
- **Input**: Search term (min 2 characters)
- **Output**: Matching products ranked by relevance
- **Validation**:
  - Search term sanitized to prevent injection
  - Special characters handled appropriately
- **Business Rules**:
  - Search across product name, band name, description
  - Case-insensitive matching
  - Partial word matching supported
  - Search results debounced (300ms delay)

### FR2.4: Product Filtering

- **Requirement**: The system shall allow users to filter products by multiple criteria
- **Input**: Size, color, price range, category
- **Output**: Filtered product list matching all selected criteria
- **Validation**:
  - Price range values must be positive
  - Multiple filters can be applied simultaneously
- **Business Rules**:
  - Filter counts show available products for each option
  - Filter state persists in URL for sharing
  - Filters applied with AND logic

### FR2.5: Product Categories

- **Requirement**: The system shall organize products into music genre categories
- **Input**: Category slug (e.g., "rock", "metal", "pop")
- **Output**: Products filtered to selected category
- **Validation**:
  - Category must exist
- **Business Rules**:
  - Categories: Rock, Metal, Pop, Indie, Hip-Hop, Electronic, Alternative, Classic Rock
  - Each product belongs to one primary category
  - Category hierarchy supported (future enhancement)

---

## FR3: Shopping Cart Management

### FR3.1: Add to Cart

- **Requirement**: The system shall allow users to add products to their shopping cart
- **Input**: Product variant ID, quantity
- **Output**: Item added to cart, cart badge updated
- **Validation**:
  - Product variant must exist and be active
  - Quantity must be positive integer
  - Stock availability checked
- **Business Rules**:
  - Maximum quantity per item: 10 or available stock (whichever is lower)
  - Duplicate items (same variant) increase existing quantity
  - Guest carts stored for 7 days
  - Logged-in user carts stored for 30 days

### FR3.2: View Cart

- **Requirement**: The system shall display all items in the user's cart
- **Input**: Cart ID (from session or user account)
- **Output**: Cart items with image, name, size, color, price, quantity, subtotal
- **Validation**:
  - Cart must exist
- **Business Rules**:
  - Cart totals include subtotal and estimated tax
  - Out-of-stock items flagged with warning
  - Empty cart shows message and "Continue Shopping" link

### FR3.3: Update Cart Quantity

- **Requirement**: The system shall allow users to modify item quantities in cart
- **Input**: Cart item ID, new quantity
- **Output**: Updated cart with recalculated totals
- **Validation**:
  - Quantity between 1 and min(stock, 10)
  - Stock availability validated
- **Business Rules**:
  - Optimistic UI updates with rollback on error
  - API calls debounced (500ms)
  - Zero quantity triggers removal confirmation

### FR3.4: Remove from Cart

- **Requirement**: The system shall allow users to remove items from cart
- **Input**: Cart item ID
- **Output**: Item removed, cart totals recalculated
- **Business Rules**:
  - Undo option available for 5 seconds
  - Soft delete during undo window

### FR3.5: Cart Persistence

- **Requirement**: The system shall persist cart data across sessions
- **Business Rules**:
  - Guest carts stored in local storage (7 days)
  - Logged-in user carts stored in database (30 days)
  - Guest cart merged with user cart on login
  - Expired carts automatically cleaned up
  - Stock revalidated when cart loaded

---

## FR4: Checkout & Payment

### FR4.1: Shipping Address Entry

- **Requirement**: The system shall collect shipping address information
- **Input**: Full name, address line 1, address line 2 (optional), city, state, postal code, country, phone
- **Output**: Validated shipping address
- **Validation**:
  - All required fields must be provided
  - Postal code format validated by country
  - Phone number format validated
- **Business Rules**:
  - Address validation via postal service API
  - Logged-in users can save addresses for future use
  - Logged-in users can select from saved addresses
  - Autocomplete suggestions provided

### FR4.2: Payment Processing

- **Requirement**: The system shall process credit card payments securely via Stripe
- **Input**: Stripe Payment Method ID (from Stripe Elements)
- **Output**: Payment confirmation or error message
- **Validation**:
  - Payment amount matches cart total
  - Card details validated by Stripe
- **Business Rules**:
  - PCI compliance via Stripe (no card data stored locally)
  - 3D Secure authentication when required
  - Payment Intent flow with webhooks
  - Idempotency keys prevent duplicate charges
  - Supported cards: Visa, Mastercard, Amex, Discover

### FR4.3: Order Creation

- **Requirement**: The system shall create an order after successful payment
- **Input**: Cart ID, shipping address, payment confirmation
- **Output**: Order with unique order number
- **Validation**:
  - Payment must be confirmed
  - Cart must not be empty
  - All items must be in stock
- **Business Rules**:
  - Order number format: ORD-YYYYMMDD-{random 6 chars}
  - Prices locked at checkout time
  - Inventory decremented atomically with order creation
  - Tax calculated based on shipping address
  - Order creation is idempotent

### FR4.4: Order Confirmation

- **Requirement**: The system shall send order confirmation to customer
- **Input**: Order ID
- **Output**: Confirmation email sent, cart cleared
- **Business Rules**:
  - Email includes order details, items, shipping address, total
  - Confirmation page displayed with order summary
  - Order tracking link included in email

### FR4.5: Guest Checkout

- **Requirement**: The system shall allow guest users to complete purchases without account
- **Input**: Guest email address, shipping information, payment
- **Output**: Order created with guest email (no user_id)
- **Validation**:
  - Email format validated
- **Business Rules**:
  - Guest orders tracked via email + order number
  - Option to create account after purchase
  - Guest order lookup available via email + order number

---

## FR5: Order Management

### FR5.1: Order History

- **Requirement**: The system shall display a list of user's past orders
- **Input**: User ID, optional filters (status, date range)
- **Output**: Paginated list of orders (20 per page)
- **Validation**:
  - User must be authenticated
- **Business Rules**:
  - Orders sorted by date (newest first)
  - Each order shows: number, date, total, status
  - Search by order number or product name
  - Filter by order status

### FR5.2: Order Details

- **Requirement**: The system shall display complete information for a specific order
- **Input**: Order ID
- **Output**: Order items, quantities, prices, shipping address, payment info, status history
- **Validation**:
  - Order must belong to authenticated user (or match guest email + order number)
- **Business Rules**:
  - Print-friendly layout
  - Download receipt/invoice option
  - Contact support link
  - Track shipment link (when shipped)

### FR5.3: Order Status Tracking

- **Requirement**: The system shall track and display order status progression
- **Input**: Order ID
- **Output**: Current status, status history with timestamps, estimated delivery
- **Business Rules**:
  - Status values: Processing, Confirmed, Shipped, Delivered, Cancelled, Returned
  - Status timeline visualization
  - Email notifications on status changes
  - Tracking number and carrier info (when shipped)

### FR5.4: Order Cancellation

- **Requirement**: The system shall allow users to cancel orders before shipment
- **Input**: Order ID, cancellation reason (optional)
- **Output**: Order status changed to Cancelled, refund initiated
- **Validation**:
  - Order must be in "Processing" status
  - User must own the order
- **Business Rules**:
  - Cancellation only allowed before "Shipped" status
  - Full refund to original payment method
  - Refund processes within 5-7 business days
  - Inventory restored for cancelled items
  - Cancellation confirmation email sent

### FR5.5: Reorder

- **Requirement**: The system shall allow users to reorder previous purchases
- **Input**: Order ID
- **Output**: All order items added to cart
- **Validation**:
  - Order must belong to user
  - Products must still exist and be active
- **Business Rules**:
  - Stock validated before adding to cart
  - Out-of-stock items excluded with notification
  - Price changes shown if applicable
  - Discontinued items excluded with notification

### FR5.6: Guest Order Tracking

- **Requirement**: The system shall allow guest users to track orders via email and order number
- **Input**: Email address, order number
- **Output**: Order status and details
- **Validation**:
  - Email and order number must match
- **Business Rules**:
  - Rate limiting: 10 lookups per IP per hour
  - Same information shown as authenticated user order details
  - Option to create account to save order history

---

## FR6: Admin Dashboard

### FR6.1: Admin Authentication

- **Requirement**: The system shall authenticate admin users with role-based access control
- **Input**: Email, password
- **Output**: Admin session with role permissions
- **Validation**:
  - User must have "admin" or "super_admin" role
- **Business Rules**:
  - Admin sessions expire after 8 hours of inactivity
  - All admin actions logged for audit trail
  - Regular users cannot access admin routes

### FR6.2: Dashboard Overview

- **Requirement**: The system shall display key business metrics on admin dashboard
- **Input**: Date range (optional)
- **Output**: Total orders, revenue, customers, products, recent orders, low stock alerts
- **Business Rules**:
  - Metrics cached for 5 minutes
  - Charts for orders and revenue over time (7 days, 30 days)
  - Click metrics to drill down
  - Export data to CSV

### FR6.3: Product Management

- **Requirement**: The system shall allow admins to create, read, update, delete products
- **Input**: Product details (name, band, description, category, price, images, variants)
- **Output**: Product created/updated/deleted
- **Validation**:
  - All required fields must be provided
  - Price must be positive
  - Category must exist
- **Business Rules**:
  - Image upload with optimization and resizing
  - Rich text editor for descriptions
  - Variant management (sizes, colors, SKUs, stock)
  - Bulk operations supported
  - Product activation/deactivation (soft delete)

### FR6.4: Inventory Management

- **Requirement**: The system shall allow admins to manage product inventory levels
- **Input**: Product variant ID, stock quantity, adjustment reason
- **Output**: Updated stock level, inventory adjustment logged
- **Validation**:
  - Stock quantity must be non-negative integer
- **Business Rules**:
  - Low stock alerts when stock < 10
  - Stock history logged with reasons
  - Bulk stock updates via CSV import
  - Daily inventory alert emails

### FR6.5: Order Management

- **Requirement**: The system shall allow admins to view and manage all orders
- **Input**: Optional filters (status, date range, customer)
- **Output**: Paginated list of all orders
- **Business Rules**:
  - Search by order number or customer email
  - Update order status
  - Add internal notes to orders
  - Process refunds
  - Print packing slips
  - Export orders to CSV

### FR6.6: Customer Management

- **Requirement**: The system shall allow admins to view and manage customer accounts
- **Input**: Optional filters (registration date, order count)
- **Output**: Paginated list of customers
- **Business Rules**:
  - View customer details and order history
  - Customer lifetime value displayed
  - Enable/disable customer accounts
  - Search by name or email
  - Export customer list to CSV

---

## FR7: Additional Features (Phase 1+)

### FR7.1: Product Reviews (Phase 1)

- **Requirement**: The system shall allow customers to leave reviews and ratings
- **Input**: Order ID, product ID, rating (1-5 stars), review text, optional photos
- **Output**: Review created and displayed on product page
- **Validation**:
  - User must have purchased the product
  - Rating must be 1-5 stars
  - Review text max 2000 characters
- **Business Rules**:
  - Only verified purchases can review
  - One review per product per user
  - Reviews can be edited within 7 days
  - Admins can moderate/remove reviews
  - Helpfulness voting enabled

### FR7.2: Wishlist (Phase 1)

- **Requirement**: The system shall allow users to save products to a wishlist
- **Input**: Product ID
- **Output**: Product added to wishlist
- **Validation**:
  - User must be logged in
  - Product must exist and be active
- **Business Rules**:
  - Wishlist persists across sessions
  - Move items from wishlist to cart
  - Share wishlist via link
  - Email notifications on price drops
  - Remove items from wishlist

### FR7.3: Promotions & Discounts (Phase 1)

- **Requirement**: The system shall support promotional codes and discounts
- **Input**: Promo code
- **Output**: Discount applied to cart total
- **Validation**:
  - Promo code must exist and be active
  - Minimum purchase requirements met
  - Expiration date not passed
- **Business Rules**:
  - Percentage and fixed-amount discounts
  - Product-specific and site-wide promotions
  - Usage limits (total uses, per-user uses)
  - Automatic discount application
  - One promo code per order

---

## Data Validation Standards

### Input Sanitization

- All user inputs sanitized to prevent XSS attacks
- HTML stripped from text fields (except rich text editor with whitelist)
- SQL injection prevented via parameterized queries (SQLAlchemy)

### Error Handling

- Clear, user-friendly error messages
- Technical details logged but not exposed to users
- Validation errors include specific field information
- HTTP status codes used appropriately:
  - 200: Success
  - 201: Created
  - 400: Bad Request (validation error)
  - 401: Unauthorized
  - 403: Forbidden
  - 404: Not Found
  - 409: Conflict (duplicate resource)
  - 422: Unprocessable Entity (semantic error)
  - 500: Internal Server Error

### Data Formats

- **Dates**: ISO 8601 format (YYYY-MM-DDTHH:mm:ssZ)
- **Currency**: USD with 2 decimal places
- **Email**: RFC 5322 compliant
- **Phone**: E.164 format recommended
- **URLs**: Properly encoded and validated

---

## Business Rules Summary

### Pricing & Payments

- All prices in USD for MVP
- Tax calculated based on shipping address
- Shipping costs calculated per order
- Free shipping threshold configurable

### Inventory Management

- Real-time stock checking
- Stock reserved during checkout (15-minute hold)
- Low stock alerts at configurable threshold
- Out-of-stock products not purchasable

### User Accounts

- Email verification required
- Password requirements enforced
- Account lockout after failed attempts
- Session management with JWT tokens

### Order Processing

- Orders processed immediately on payment success
- Order numbers unique and sequential
- Order prices locked at checkout
- Order modification only by admin after creation

### Data Retention

- Active user data: Retained indefinitely (until account deletion)
- Order history: 7 years (accounting/legal requirement)
- Inactive accounts: 3 years before deletion
- Guest orders: 2 years
- Cart data: 30 days for users, 7 days for guests

---

**Document Status**: ✅ Complete  
**Last Updated**: November 22, 2024  
**Version**: 1.0  
**Owner**: Product Owner

**Related Documents**:

- [Non-Functional Requirements](./non-functional-requirements.md)
- [Technical Architecture](../architecture/technical-architecture.md)
- [Epics & User Stories](../user-stories/epics-and-user-stories.md)
