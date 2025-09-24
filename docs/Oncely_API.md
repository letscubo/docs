## Introduction

### Partner Notification Profile

> You need to collaborate with the Oncely Ops team to complete the partner communication form.  
After submitting the partner communication form, the Partner Notification Profile will be generated within 24 hours.  
The partner needs to provide relevant information, including the Notify URL and Authorization Bearer `${token}`.  
Oncely will make HTTP calls (format: `application/json`) to the provided URL with the token when necessary.

#### Oncely makes calls based on different order status scenarios: Order Creation / Order Refund / Subscription Creation / Subscription Cancellation / Subscription Activation.

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


<br/>

**Order Creation**

> When a user purchases a regular product on Oncely, Oncely creates an order and sends an HTTP request for **Order Creation** to the Partner.

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

**Response 200 OK**
```json
{
  "message": "ok"
}
```
<br/><br/>

**Subscription Creation**

> When a user purchases a subscription product on Oncely, Oncely creates an order and sends an HTTP request for **Subscription Creation** to the Partner.

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

**Response 200 OK**
```json
{
  "message": "ok"
}
```
  

  
