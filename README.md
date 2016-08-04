# Getting Started with Monetary Pay API
###Authorization

Authorization is easy, just insert your secret key in the `Authorization` header:

`Authorization: secretKEYGOESHERE`
  
###Content Types
Communicate with us in your favorite content type!

We support the following for `Content-Type` and `Accepts` values:

* `application/json`
* `application/xml`
* `x-www-url-encoded`

##Transactions

###[Credit Transactions](../master/CREDIT.md)
* Sale `POST /credit/sale`
* Void Sale `POST /credit/sale/:RefNo/void`
* Adjust `PUT /credit/sale/:RefNo`
* Return `POST /credit/return`
* Void Return `POST /credit/return/:RefNo/void`
* Auth Only: `POST /credit/authonly`

###Success Responses
* ```200 OK``` Approved Transaction
* ```402 PAYMENT REQUIRED``` Declined Transaction

###Failure Responses
* ```400 BAD REQUEST``` Invalid Transaction Request
* ```401 UNAUTHORIZED``` Unauthorized Transaction
* ```404 NOT FOUND``` Resource Not Found

###Example Sale Request

```
POST https://pay.monetary.co/v1/credit/sale

Authorization: secretKEYGOESHERE
Content-Type: application/json
Accept: application/json

{
  "Purchase": "1.00",
  "Account": "4242424242424242",
  "Expiration": "1220"
}
```

###Example Sale Response
```
200 OK

{
  "Status": "Approved",
  "Message": "APPROVAL",
  "Account": "XXXXXXXXXXXX4242",
  "Expiration": "XXXX",
  "Brand": "VISA",
  "RefNo": "123",
  "Token": "card_1ABCDEFG2"
}
```

##Using Tokens
As you can see in the [example response above](#example-sale-response), every successful transaction response will include a `Token` which you can use in subsequent transactions for that account!

For example, this is how we void the above example sale using the `RefNo` and `Token` it returned:

###Example Void Request with Token

```
POST https://pay.monetary.co/v1/credit/sale/123/void

Authorization: secretKEYGOESHERE
Content-Type: application/json
Accept: application/json

{
  "Token": "card_1ABCDEFG2"
}
```

###Example Void Response with Token
```
200 OK

{
  "Status": "Approved",
  "Message": "APPROVAL",
  "Account": "XXXXXXXXXXXX4242",
  "Expiration": "XXXX",
  "Brand": "VISA",
  "RefNo": "124",
  "Token": "card_1ABCDEFG2"
}
```
