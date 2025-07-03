## Introduction

### Partner Notification Profile

> You need to collaborate with the Oncely Ops team to complete the partner communication form.  
After submitting the partner communication form, the Partner Notification Profile will be generated within 24 hours.  
The partner needs to provide relevant information, including the Notify URL and Authorization Bearer `${token}`.  
Oncely will make HTTP calls (format: `application/json`) to the provided URL with the token when necessary.

#### Oncely makes calls based on different order status scenarios: Order Creation / Order Refund / Subscription Creation / Subscription Cancellation / Subscription Activation.

1. **Order Creation**  
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
| `userInfo`        | User information for service activation (JSON string) e.g., `{"email":"xx@xx.com", "password":"***"}` |

2. **Order Refund**  
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

3. **Subscription Creation**  
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
| `userInfo`        | User information for service activation (JSON string) e.g., `{"email":"xx@xx.com", "password":"***"}` |

4. **Subscription Cancellation**  
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

5. **Subscription Activation**  
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