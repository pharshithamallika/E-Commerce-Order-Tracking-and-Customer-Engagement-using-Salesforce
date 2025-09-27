# E-Commerce-Order-Tracking-and-Customer-Engagement-using-Salesforce
Phase 1: Problem Understanding & Industry Analysis Goal

To understand the challenges e-commerce businesses face in tracking customer orders, managing delivery updates, and engaging customers before and after purchase, and how a CRM like Salesforce can make these processes seamless, transparent, and customer-focused.

Requirement Gathering:

   We receive hundreds of orders every day, but tracking statuses across different courier partners is messy.
   Customers keep calling support to ask ‘Where is my order?’ — this increases workload.
   We send shipping notifications, but they’re generic and don’t build a relationship with the customer.
   Customers want real-time updates and transparency. If an order is delayed, they want to know immediately.
   Post-delivery, we rarely follow up for feedback or reviews — we miss chances to build loyalty.

So, the system must:

   Track all customer orders and shipment statuses.
   Automate real-time notifications via email, SMS, or WhatsApp.
   Provide a single view of each customer’s order history.
   Enable service agents to resolve issues quickly.
   Gather feedback and re-engage customers with offers and recommendations.

Stakeholder Analysis:

   E-Commerce Admin: Sets up Salesforce, integrates with the e-commerce platform, and ensures smooth functioning.
   Customer Support Agent: Needs quick access to customer + order history to answer queries and resolve issues.
   Marketing Team: Wants to personalize customer journeys with targeted promotions, feedback requests, and re-engagement campaigns.
   Logistics/Operations Manager: Needs visibility into shipment statuses, delays, and delivery SLAs.
   Customers: Expect real-time updates, transparency, and smooth communication throughout their purchase journey.

Business Process Mapping:

   A customer places an order on the e-commerce website.
   Order details are captured in Salesforce (Order object + Products).
   Salesforce triggers an order confirmation email/SMS.
   When logistics update shipment → Salesforce updates order status.
   Customer automatically receives updates (Shipped, Out for Delivery, Delivered).
   After delivery, a feedback survey or rating request is sent.
   Marketing Cloud can follow up with personalized recommendations or offers.
   Dashboards in Salesforce/CRM Analytics track delivery performance, delays, and engagement.

Industry-Specific Use Case Analysis:

   Customers expect Amazon-like transparency: real-time updates and proactive notifications.
   Trust is built when businesses are transparent about delays and responsive to issues.
   Order tracking is not enough — businesses must also re-engage customers post-purchase with loyalty campaigns, reviews, or product suggestions.
   Without continuous engagement, repeat purchases decline and customer churn increases.

AppExchange Exploration:

  There are existing Salesforce solutions for retail and e-commerce (e.g., Commerce Cloud, Marketing Cloud, Service Cloud integrations). For learning purposes, we will build a simplified custom CRM with objects     for:

  Orders,
  Order Items,
  Customers,
  Notifications,
  Feedback & Engagement

Phase 2: Org Setup & Configuration for E-Commerce Order Tracking & Customer Engagement

 Salesforce Editions
   We used Salesforce Enterprise Edition, as it provides advanced features such as Order Management, Service Cloud, and Marketing Cloud integration. This ensures scalability for e-commerce operations.
 Company Profile Setup
   Configured the company profile for an e-commerce store (e.g., “SmartCart Pvt Ltd”) with details such as company name, currency (INR), locale, time zone, and contact information.
 Business Hours & Holidays
   Defined business hours (Mon–Sat, 9:00 AM – 9:00 PM) for customer service teams handling order-related queries. Added holiday calendars (e.g., Diwali, Christmas) to ensure SLA timelines are calculated             correctly.
 Fiscal Year Settings
   Configured the fiscal year starting in April, aligning with Indian e-commerce financial cycles. This supports accurate reporting and revenue analysis.
 User Setup & Licenses
   Created users with roles such as:
   Customer Support Agent (handles order queries)
   Sales Manager (monitors sales and order trends)
   Marketing Specialist (customer engagement campaigns)
   Assigned appropriate Salesforce licenses (Salesforce Platform, Service Cloud, Marketing Cloud).
Login Access Policies
   Configured login policies to secure customer data. Enabled MFA (Multi-Factor Authentication) for internal users. Restricted login IP ranges for security.
Dev Org Setup
   Created a Salesforce Developer Org to implement and test e-commerce order tracking flows and automation before deploying to production.
Sandbox Usage
   Configured a Full Sandbox for testing real order data and customer engagement campaigns. A Developer Sandbox was also used for individual testing and prototyping.
Deployment Basics
   Implemented change sets to move custom objects (e.g., Order__c, Shipment__c), workflows, and automation rules from sandbox to production.
Use Case Explanations
  For each configuration:
  Company Profile Setup → Helps align Salesforce org with the company’s e-commerce brand identity.
  Business Hours & Holidays → Ensures customers receive accurate delivery timelines.
  Sandbox Usage → Prevents disruptions to live customer orders while testing.

 Phase 3: Data Modeling & Relationships
  1. Standard & Custom Objects
     Standard Objects Used:
      Account – represents customers (buyers).
      Contact – customer details (name, email, phone).
      Order – standard Salesforce object for order management.
      Case – for order issues or complaints.
   Custom Objects Created:
      Product__c – stores e-commerce product catalog.
      Shipment__c – tracks delivery/shipping details.
      Engagement__c – stores customer engagement activities like feedback, loyalty points, offers.
 2. Fields
    Custom Fields Added:
      On Order → Order_Status__c (Picklist: Pending, Shipped, Delivered, Cancelled)
      On Shipment__c → Tracking_ID__c (Text), Delivery_Date__c (Date)
      On Engagement__c → Reward_Points__c (Number), Feedback__c (Long Text Area)
 3. Record Types
    Order Object Record Types:
      B2C Order – for individual customers.
      B2B Order – for wholesale / bulk orders.
 4. Page Layouts
      Designed Order Layout with fields like Product, Quantity, Status, Payment Mode.
      Shipment Layout includes Tracking ID, Courier Partner, Delivery Date.
 5. Compact Layouts
      Order Compact Layout: Shows Order Number, Status, Amount.
      Shipment Compact Layout: Shows Tracking ID, Delivery Date, Status.
 6. Schema Builder
      Used Schema Builder to visualize relationships between Account → Order → Shipment and Account → Engagement.
 7. Lookup vs Master-Detail vs Hierarchical Relationships
      Lookup Relationship: Order → Shipment (each order can have multiple shipments).
      Master-Detail Relationship: Order → Engagement (engagement records depend on order).
      Hierarchical Relationship: Used in User object for Manager–Agent hierarchy.
 8. Junction Objects
      Created OrderProduct__c (junction between Order and Product) to allow many products in a single order.
 9. External Objects
      Configured External Object for Payment Gateway Integration (e.g., PayPal / Razorpay transaction history).
      Fields: Transaction_ID__x, Payment_Status__x, Payment_Date__x.

Phase 4: Process Automation (Admin)
 1. Validation Rules
     Use Case: Ensure order data is valid before saving.
     Example: Prevent saving an order if Quantity < 1.
     Formula: Quantity__c < 1 → Error: “Quantity must be at least 1.”
 2. Workflow Rules
     Use Case: Auto-send an email when an order status changes to Shipped.
     Action: Email alert to the customer with tracking details.
 3. Process Builder
     Use Case: When an order is created, automatically create a related shipment record.
    Example: Order → Creates Shipment__c with default “Pending Dispatch” status.
 4. Approval Process
     Use Case: Bulk orders (> ₹50,000) require manager approval before processing.
     Steps:
      Sales Agent submits order for approval.
      Manager approves/rejects.
 5. Flow Builder
     Screen Flow → For customers entering feedback after delivery.
     Record-Triggered Flow → Auto-update shipment status when delivery date is reached.
     Scheduled Flow → Send reminder emails for pending payments every 3 days.
     Auto-launched Flow → Award loyalty points once order status = Delivered.
 6. Email Alerts
     Use Case: Send delivery confirmation email when order status changes to Delivered.
 7. Field Updates
     Use Case: Update Customer_Status__c field to “Active” when first order is placed.
 8. Tasks
     Use Case: Create a follow-up task for support agent if a customer raises a complaint (Case record).
 9. Custom Notifications
     Use Case: Send a push notification to sales managers when high-value orders (> ₹1,00,000) are placed.

Phase 5: Apex Programming (Developer)
 1. Classes & Objects
     Created helper classes to handle reusable business logic (e.g., updating order status, processing shipments).
     Helps keep the project modular and easier to maintain.
 2. Apex Triggers (before/after insert/update/delete)
     Implemented triggers on the Order object to auto-create Shipment records when an order is placed.
     Triggers also update engagement records when order status changes to Delivered.
 3. Trigger Design Pattern
     Used Trigger Handler approach to separate logic from triggers.
     This ensures scalability and avoids writing bulky triggers.
 4. SOQL & SOSL
     SOQL was used to fetch order, shipment, and engagement data for reports.
     SOSL was used for searching customers by name, email, or phone.
 5. Collections: List, Set, Map
     List used for handling multiple shipment records at once.
     Set used to ensure unique tracking IDs.
     Map used to relate orders with their shipments.
6. Control Statements
     Implemented if/else, for loops, and switch cases to manage order lifecycle conditions (Pending, Shipped, Delivered, Cancelled).
7. Batch Apex
     Used to archive old orders (e.g., older than one year).
     Ensures large volumes of data can be processed in batches.
8. Queueable Apex
     Implemented to process bulk customer feedback asynchronously, without impacting system performance.
9. Scheduled Apex
     Used to send daily order summary reports to managers at a scheduled time (9 AM).
10. Future Methods
     Implemented to perform external API callouts (e.g., fetching real-time courier tracking updates).
11. Exception Handling
     Standardized error handling to ensure that invalid data or system failures are logged and do not interrupt processes.
12. Test Classes
     Created test classes to achieve 75%+ test coverage, ensuring all triggers, classes, and methods work correctly before deployment.
13. Asynchronous Processing
     Implemented a mix of Batch Apex, Queueable Apex, Scheduled Apex, and Future Methods to handle:
     Bulk data processing (orders, shipments).
     Real-time customer engagement.
     Automated daily reporting.
     Third-party integrations (e.g., payment gateways, courier tracking).    
    


