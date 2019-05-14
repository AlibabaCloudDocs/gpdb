# Expiration and overdue policy {#concept_pc5_z5t_2gb .concept}

If a Subscription instance expires or a Pay-As-You-Go instance is overdue, the instance will be locked for a period and then be released automatically. No data can be recovered after the instance is released.

|Billing method|Status|Operation|
|--------------|------|---------|
|Subscription|From the 1st day to the 7th day after the instance expires, the instance is locked and cannot be accessed.|Manually renew the instance, and the instance will immediately become normal.|
|On the 7th day after the instance expires, the instance is released.|The data is completely deleted and cannot be recovered.|
|Pay-As-You-Go| If you exceed the credit limit of your credit card to which your Alibaba Cloud account is bound, all the instances under this account will be in the status of overdue.

 On the 1st day after the instance is overdue, the instance works normally.

 From the 2nd day to the 7th day after the instance is overdue, the instance is locked and cannot be accessed.

 |Recharge the Alibaba Cloud account and the instance will become normal immediately.|
|On the 8th day after the instance is overdue, the instance is released.|The data is completely deleted and cannot be recovered.|

