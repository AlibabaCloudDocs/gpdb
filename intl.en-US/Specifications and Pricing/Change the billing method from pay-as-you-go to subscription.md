# Change the billing method from pay-as-you-go to subscription

This topic describes how to change the billing method of an AnalyticDB for PostgreSQL instance from pay-as-you-go to subscription after you purchase that instance.

## Precautions

-   You cannot change the billing method from subscription to pay-as-you-go. To optimize your cost plan, we recommend that you evaluate your usage model carefully before you change the billing method to subscription.
-   You can upgrade the specifications of your subscription instance during the subscription period. However, you cannot release your subscription instance nor can you downgrade its specifications.
-   Subscription billing immediately takes effect after you change the billing method of your instance from pay-as-you-go to subscription.
-   After you change the billing method of your instance from pay-as-you-go to subscription, the system generates a subscription order. Subscription billing takes effect after you pay the subscription fee. If you do not pay for this order, an unpaid order is displayed on the [Orders](https://expense.console.aliyun.com/?/order/list/) page. In this situation, you cannot purchase a new instance or change the billing method of another instance until you pay the unpaid balance.

## Prerequisites

-   The billing method of the instance is pay-as-you-go and the instance is in the **Running** state.
-   The instance does not have an unpaid balance.

## Procedure

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  Find the target instance and click **Subscription Billing** in the Action column.
3.  On the order confirmation page, specify a **Purchase Cycle**, read and select Agreement of Service, and click **Activate**.
4.  On the Orders page, click **Confirm Payment**.

**Note:** You must complete the subscription order generated for the instance. You cannot purchase a new instance or change the billing method of another instance until you pay for or cancel this order. You can pay for or cancel this order on the [Billing Management](https://expense.console.aliyun.com/#/order/list/) page.

