---
layout: default
group: b2b
subgroup: 10_REST
title: Manage company credit
menu_title: Manage company credit
menu_order: 18
level3_menu_node: level3child
level3_subgroup: credit
version: 2.2
ee_only: true
github_link: b2b/credit-manage.md
---

The company credit entity operates with the following attributes:

* Credit limit
* Available credit
* Outstanding balance

The credit limit is allocated by merchant, while available credit and outstanding balance are automatically calculated by the system based on the customer transactions (place an order, return) and merchant's transactions (refund, reimburse, update credit limit, cancel order).

## Manage company credit limits

When you create a company, the credit limit is set to 0. Use the `PUT /V1/companyCredits/:id` call to change this value and perform other updates to the company's credit settings.

**REST Endpoints**

{% highlight json %}
PUT /V1/companyCredits/:id
GET /V1/companyCredits/:creditId
GET /V1/companyCredits/company/:companyId
GET /V1/companyCredits/
{% endhighlight %}

**Company credit parameters**

Name | Description | Format | Requirements
--- | --- | --- | ---
`id` | The credit ID generated by the system | integer | Required
`company_id` | Company ID | integer | Required
`credit_limit` | The amount of credit granted to the company | Number | Required
`balance` | The amount the company currently owes the merchant | Number | Optional
`currency_code` | The currency code for the company's credit, such as USD | String | Required
`exceed_limit` | Indicates whether the company can exceed their credit limit | Boolean  | Optional
`available_limit` | The amount of credit currently available to the company | Number | Optional
`credit_comment` | Describes the change being made | String | Optional

### Update a company credit limit

This call changes the company's credit limitto $1000. The `available_limit` parameter is calculated, so you cannot specify the value.

**Service Name**

`companyCreditCreditLimitRepositoryV1`

**Sample Usage**

`PUT /V1/companyCredits/company/2`

**Payload**

{% highlight json %}
{
  "creditLimit": {
  "id": 2,
  "company_id": 2,
  "credit_limit": 1000,
  "currency_code": "USD"
  }
}
{% endhighlight %}

**Response**

{% highlight json %}
{
    "id": 2,
    "company_id": 2,
    "credit_limit": 1000,
    "balance": 0,
    "currency_code": "USD",
    "exceed_limit": false,
    "available_limit": 1000
}
{% endhighlight %}

### Get details about a company's credit limit using credit ID

This call returns data on the credit limit for the specified credit ID.

**Service Name**

`companyCreditCreditLimitRepositoryV1`

**Sample Usage**

`GET /V1/companyCredits/2`

**Payload**

Not applicable

**Response**

{% highlight json %}
{
  "id": 2,
  "company_id": 2,
  "credit_limit": 500,
  "balance": 0,
  "currency_code": "USD",
  "exceed_limit": false,
  "available_limit": 500
}
{% endhighlight %}

### Get details about a company's credit limit using company ID

This call returns information about the credit limit for a specified company.

**Service Name**

`companyCreditCreditLimitManagementV1`

**Sample Usage**

`GET /V1/companyCredits/company/2`

**Payload**

Not applicable

**Response**

{% highlight json %}
{
  "id": 2,
  "company_id": 2,
  "credit_limit": 500,
  "balance": 0,
  "currency_code": "USD",
  "exceed_limit": false,
  "available_limit": 500
}
{% endhighlight %}

### Search for credit IDs

The `GET  /V1/companyCredits/` call returns a list of credits for companies that match the specified criteria. See [Search using REST APIs]({{page.baseurl}}howdoi/webapi/search-criteria.html) for information about constructing a query using the this endpoint.


## Balance operations

The company's outstanding balance can be updated as the merchant makes payments, purchases, and other transactions.

**Service Name**

`companyCreditCreditBalanceManagementV1`

**REST Endpoints**

{% highlight json %}
POST /V1/companyCredits/:creditId/decreaseBalance
POST /V1/companyCredits/:creditId/increaseBalance
{% endhighlight %}

**Balance Parameters**

Name | Description | Format | Requirements
--- | --- | --- | ---
`value` | Indicates how much money is involved in this company credit balance operation. | Nuumber | Required
`currency` | The currency of the transaction, such as USD | String | Required
`operationType` | Must be one of the following: 1 - Allocated; 2 - Updated; 3 - Purchased; 4 - Reimbursed; 5 - Refunded; 6 - Reverted | Integer | Required
`comment` | Describers the operation | String | Optional
`options` | An object that provides additional information for increasing or decreasing the credit balance | Object | Optional

**`options` parameters**

Name | Description | Format | Requirements
--- | --- | --- | ---
`purchase_order` | The company's purchase order number  | String | Optional
`order_increment` | Order increment | String | Optional
`currency_display` | Currency code for displaying the operation | String | Optional
`currency_base` | The base currency |  Optional

### Increase the company credit balance

This call increases the company credit with an Allocate, Update, Refund, Revert, or Reimburse transaction. (You cannot specify the Purchased (3) operation type.) This call also decreases the company's outstanding balance.

**Sample Usage**

`V1/companyCredits/2/increaseBalance`

**Payload**

{% highlight json %}
{
  "value": 250,
  "currency": "USD",
  "operationType": 2,
  "comment": "update limit"
}
{% endhighlight %}

**Response**

`true`, indicating the increase to the company credit balance succeeded

### Decrease the balance

This call decreases the company credit with an Update (operation type = 2), Purchased (3), or Reimbursed (4) transaction. (You cannot specify the other operation types.) This call also increases company's outstanding balance.

**Sample Usage**

`V1/companyCredits/2/decreaseBalance`

**Payload**

{% highlight json %}
{
  "value": 250,
  "currency": "USD",
  "operationType": 4,
  "comment": "issue refund"
}
{% endhighlight %}

**Response**

`true`, indicating the decrease to the company credit balance succeeded


## Credit history

A Reimburse transaction can be updated to include a purchase order and comment.

**Service Name**
`companyCreditCreditHistoryManagementV1`

**REST Endpoints**
{% highlight json %}
GET /V1/companyCredits/history
PUT /V1/companyCredits/history/:historyId
{% endhighlight %}

### Save the credit history

This call updates the credit history to specify a purchase order number.

**Sample Usage**

`PUT /V1/companyCredits/history/6`

**Payload**

{
  "purchaseOrder": "A12345",
  "comment": "Adding PO info"
}

**Response**

`true`, indicating the call was successful

### Search credit history IDs

The `GET  /V1/companyCredits/history` call returns a list of history IDs for companies that match the specified criteria. See [Search using REST APIs]({{page.baseurl}}howdoi/webapi/search-criteria.html) for information about constructing a query using the this endpoint.

## Related information

[Integrate with the CompanyCredit module]({{page.baseurl}}/b2b/company-credit.html)
