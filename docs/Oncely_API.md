## Introduction

### Partner Notification Profile

> You need to collaborate with the Oncely Ops team to complete the partner communication form.  
After submitting the partner communication form, the Partner Notification Profile will be generated within 24 hours.  
The partner needs to provide relevant information, including the Notify URL and Authorization Bearer `${token}`.  
Oncely will make HTTP calls (format: `application/json`) to the provided URL with the token when necessary.

#### Oncely makes calls based on different order status scenarios: Order Creation / Order Refund / Subscription Creation / Subscription Cancellation / Subscription Activation.

<div class="mermaid">

flowchart TD
    A[User Action] --> B{Action Type Determination}

    B -->|Purchase Regular Product| C[Order Creation]
    B -->|Request Refund| D[Order Refund]
    B -->|Purchase Subscription Product| E[Subscription Creation]
    B -->|Cancel Subscription| F[Subscription Cancellation]
    B -->|Reactivate Subscription| G[Subscription Activation]
    
    C --> C1[Oncely Creates Order]
    C1 --> C2[Send Order Creation Request]
    C2 --> C3[Partner Processes Request]
    C3 --> C4[Return redirect_url]
    
    D --> D1[Oncely Updates Refund Status]
    D1 --> D2[Send Order Refund Request]
    D2 --> D3[Partner Processes Request]
    D3 --> D4[Return Confirmation Message]
    
    E --> E1[Oncely Creates Subscription Order]
    E1 --> E2[Send Subscription Created Request]
    E2 --> E3[Partner Processes Request]
    E3 --> E4[Return redirect_url]
    
    F --> F1[Oncely Updates Subscription Status]
    F1 --> F2[Send Subscription Cancel Request]
    F2 --> F3[Partner Processes Request]
    F3 --> F4[Return Confirmation Message]
    
    G --> G1[Oncely Updates Subscription Status]
    G1 --> G2[Send Subscription Activated Request]
    G2 --> G3[Partner Processes Request]
    G3 --> G4[Return Confirmation Message]
    
    C4 --> H[User Redirected to Activation Page]
    E4 --> H
    D4 --> I[Process Completed]
    F4 --> I
    G4 --> I
    
    %% Style Definitions
    classDef oncely fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef partner fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef user fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    
    class C1,D1,E1,F1,G1 oncely
    class C3,D3,E3,F3,G3 partner
    class A,H user

</div>
<br/>

**Order Creation**

> When a user purchases a regular product on Oncely, Oncely creates an order and sends an HTTP request for **Order Creation** to the Partner.

> Partners need to provide an activation link for users to activate after a successful purchase.
(Please return the redirect_url data in the response json)

| Parameter Name    | Parameter Value                                                  |
|-------------------|------------------------------------------------------------------|
| `action`          | `orders/create`                                                  |
| `uuid`            | Unique ID associated with the order                              |
| `email`           | Buyer's email                                                    |
| `created`         | Order creation time                                              |
| `status`          | Current order status (`paid`)                                    |
| `price`           | Order amount                                                     |
| `productId`       | Order product ID                                                 |
| `productName`     | Order product name                                               |
| `variantId`       | Order product variant ID                                         |
| `variantName`     | Order product variant name                                       |

**Request**
```json
{
  "action": "orders/create",
  "uuid": "ord_7c62c1f8a4b5d9e3f6a2b1c9",
  "email": "customer@example.com",
  "created": "2024-01-15T10:30:00Z",
  "status": "paid",
  "price": 99.99,
  "productId": "prod_123456",
  "productName": "Premium Software License",
  "variantId": "var_789012",
  "variantName": "Annual Plan"
}
```

**Response 200 OK**
```json
{
  "message": "ok",
  "redirect_url": "https://<site>/login?a=61yvd1f&source=oncely"  // Partner’s URL where activation will be completed
}
```
    
<br/><br/>

**Order Refund**

> When a user initiates a refund, Oncely updates the order refund status and sends an HTTP request for **Order Refund** to the Partner.

| Parameter Name    | Parameter Value                                                  |
|-------------------|------------------------------------------------------------------|
| `action`          | `orders/refund`                                                  |
| `uuid`            | Unique ID associated with the order                              |
| `email`           | Buyer's email                                                    |
| `created`         | Order creation time                                              |
| `status`          | Current order status (`refund`)                                  |
| `price`           | Order amount                                                     |
| `productId`       | Order product ID                                                 |
| `productName`     | Order product name                                               |
| `variantId`       | Order product variant ID                                         |
| `variantName`     | Order product variant name                                       |  

**Request**
```json
{
  "action": "orders/refund",
  "uuid": "ord_7c62c1f8a4b5d9e3f6a2b1c9",
  "email": "customer@example.com",
  "created": "2024-01-15T10:30:00Z",
  "status": "refund",
  "price": 99.99,
  "productId": "prod_123456",
  "productName": "Premium Software License",
  "variantId": "var_789012",
  "variantName": "Annual Plan"
}
```
**Response 200 OK**
```json
{
  "message": "ok"
}
```


<br/><br/>

**Subscription Creation**

> When a user purchases a subscription product on Oncely, Oncely creates an order and sends an HTTP request for **Subscription Creation** to the Partner.

> Partners need to provide an activation link for users to activate after a successful purchase.
(Please return the redirect_url data in the response json)

| Parameter Name    | Parameter Value                                                  |
|-------------------|------------------------------------------------------------------|
| `action`          | `SubscriptionCreated`                                            |
| `uuid`            | Unique ID associated with the order                              |
| `email`           | Buyer's email                                                    |
| `created`         | Order creation time                                              |
| `status`          | Current order status (`ACTIVE`)                                  |
| `price`           | Order amount                                                     |
| `productId`       | Order product ID                                                 |
| `productName`     | Order product name                                               |
| `variantId`       | Order product variant ID                                         |
| `variantName`     | Order product variant name                                       |
| `subscriptionId`  | Subscription ID                                                  |
| `planId`          | Plan ID associated with the order                                |
| `planName`        | Plan name associated with the order                              |

**Request**
```json
{
  "action": "SubscriptionCreated",
  "uuid": "sub_8d73c2g9b5e4f7a1c6b3d2a8",
  "email": "subscriber@example.com",
  "created": "2024-01-15T14:20:00Z",
  "status": "ACTIVE",
  "price": 29.99,
  "productId": "prod_789012",
  "productName": "Premium Monthly Subscription",
  "variantId": "var_345678",
  "variantName": "Monthly Plan",
  "subscriptionId": "sub_abc123def456",
  "planId": "plan_monthly_29.99",
  "planName": "Monthly Subscription Plan"
}
```
**Response 200 OK**
```json
{
  "message": "ok",
  "redirect_url": "https://<site>/login?a=61yvd1f&source=oncely"  // Partner’s URL where activation will be completed
}
```

<br/><br/>

**Subscription Cancellation**

> When a user cancels a subscription order on Oncely, Oncely updates the order and sends an HTTP request for **Subscription Cancellation** to the Partner.

| Parameter Name    | Parameter Value                                                  |
|-------------------|------------------------------------------------------------------|
| `action`          | `SubscriptionCancel`                                             |
| `uuid`            | Unique ID associated with the order                              |
| `email`           | Buyer's email                                                    |
| `created`         | Order creation time                                              |
| `status`          | Current order status (`CANCELLED`)                               |
| `price`           | Order amount                                                     |
| `productId`       | Order product ID                                                 |
| `productName`     | Order product name                                               |
| `variantId`       | Order product variant ID                                         |
| `variantName`     | Order product variant name                                       |
| `subscriptionId`  | Subscription ID                                                  |
| `planId`          | Plan ID associated with the order                                |
| `planName`        | Plan name associated with the order                              |  

**Request**
```json
{
  "action": "SubscriptionCancel",
  "uuid": "sub_8d73c2g9b5e4f7a1c6b3d2a8",
  "email": "subscriber@example.com",
  "created": "2024-01-15T14:20:00Z",
  "status": "CANCELLED",
  "price": 29.99,
  "productId": "prod_789012",
  "productName": "Premium Monthly Subscription",
  "variantId": "var_345678",
  "variantName": "Monthly Plan",
  "subscriptionId": "sub_abc123def456",
  "planId": "plan_monthly_29.99",
  "planName": "Monthly Subscription Plan"
}

```
**Response 200 OK**
```json
{
  "message": "ok"
}
```
<br/><br/>


**Subscription Activation**

> When a user reactivates a subscription order on Oncely, Oncely updates the order and sends an HTTP request for **Subscription Activation** to the Partner.

| Parameter Name    | Parameter Value                                                  |
|-------------------|------------------------------------------------------------------|
| `action`          | `SubscriptionActivated`                                          |
| `uuid`            | Unique ID associated with the order                              |
| `email`           | Buyer's email                                                    |
| `created`         | Order creation time                                              |
| `status`          | Current order status (`ACTIVE`)                                  |
| `price`           | Order amount                                                     |
| `productId`       | Order product ID                                                 |
| `productName`     | Order product name                                               |
| `variantId`       | Order product variant ID                                         |
| `variantName`     | Order product variant name                                       |
| `subscriptionId`  | Subscription ID                                                  |
| `planId`          | Plan ID associated with the order                                |
| `planName`        | Plan name associated with the order                              |    

**Request**
```json
{
  "action": "SubscriptionActivated",
  "uuid": "sub_8d73c2g9b5e4f7a1c6b3d2a8",
  "email": "subscriber@example.com",
  "created": "2024-01-15T14:20:00Z",
  "status": "ACTIVE",
  "price": 29.99,
  "productId": "prod_789012",
  "productName": "Premium Monthly Subscription",
  "variantId": "var_345678",
  "variantName": "Monthly Plan",
  "subscriptionId": "sub_abc123def456",
  "planId": "plan_monthly_29.99",
  "planName": "Monthly Subscription Plan"
}
```
**Response 200 OK**
```json
{
  "message": "ok"
}
```
  

<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true});</script>
