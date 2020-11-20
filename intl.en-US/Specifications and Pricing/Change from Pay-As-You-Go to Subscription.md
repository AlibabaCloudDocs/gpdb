# Change from Pay-As-You-Go to Subscription

Based on your needs, you can change the billing method of a purchased Pay-As-You-Go instance to Subscription.

## Note

-   You cannot change the billing method from Subscription billing to Pay-As-You-Go billing. To optimize your cost plan, evaluate your usage model carefully before you change to Subscription.
-   Subscription instances can only be upgraded within the subscription period, and cannot be downgraded or released.
-   Your switch from Pay-As-You-Go to Subscription billing takes effect immediately.
-   The system generates a new order for the Subscription billing method. You must complete the order payment to use the billing method. If the order payment is not received, an unpaid order is displayed on the [Orders](https://expense.console.aliyun.com/?/order/list/) page. In this case, you cannot purchase any instances or change the billing method.

## Prerequisites for changing the billing method

-   The billing method of the instance is Pay-As-You-Go, and the instance is in the **Running** status.
-   There are no unpaid instance orders \(a new purchase\) for changing the billing method.

## Procedure

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  Locate the target instance. In the Actions column, click **Subscription Billing**.
3.  On the page displayed, specify the **Duration**. Read and select the Product Terms of Service. Click **Pay Now**.
4.  On the Confirm Order page, click **Pay** to complete the payment.

**Note:** The system will generate an order for Subscription billing method. If this order is canceled or unpaid, you cannot purchase a new instance or change to the Subscription billing method . You can pay the order or cancel the order on the [Orders](https://expense.console.aliyun.com/#/order/list/) page.

