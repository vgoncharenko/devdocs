---
layout: default
group: b2b
subgroup: 10_REST
title: Integrate with the CompanyCredit module
menu_title: Integrate with the CompanyCredit module
menu_order: 17
version: 2.2
ee_only: true
level3_menu_node: level3child
level3_subgroup: credit
github_link: b2b/company-credit.md
---

Company credit allows company members to purchase items on credit. This is a specific B2B feature used for transactions between companies only. The merchant allocates an amount (or the credit limit) to a company and then company members can purchase items using this amount with the Payment on Account method. The credit amount used by a company is refunded to the merchant offline. Then the merchant creates a Reimburse transaction in the system to adjust the company balance.

The following diagram illustrates the process flow of orders using the Payment on Account method.

![Payment on credit]({{page.baseurl}}b2b/images/payment-on-credit.png)

## Related information

[Manage company credit]({{page.baseurl}}b2b/credit-manage.html)
