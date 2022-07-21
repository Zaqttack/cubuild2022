
# Transaction Enablement

### Required Info:


| Attribute   | Description |
| ----------- | ----------- |
| applyTo     | Accounts where to send funds to |
| experience  | Experience Type |
| PaymentAccountId | Swivel provided payment account id |
| type | "Checking", "Savings", "Loan" |
| amount | Amount of transaction |
| firstName | User’s first name |
| lastName | User’s last name |
| emailAddress | Email associated with the account | 
| address | User’s billing street address |
| city | User’s billing city |
| state | User’s state |
| zipCode | User’s zip code |


### Initialization

```js
const Payments = new SWBC.Payments({
  Mode: "MODE_SELECTION",
  IntegrationID: "INTEGRATION_ID",
  PaymentAccountID: "4_DIGIT_PAYMENT_ID",
  Element: "YOUR_ELEMENT_ID",
  Experience: "SELECTED_EXPERIENCE"
});

Payments.setPaymentResponseHandler(handler);
```

### Transaction Enabled Experiences

1. Credit Card Only 

```js
Experience: 'NAO_CC_ONLY'
```
#### Example Request Payload:
```js
{
  "experience": "NAO_CC_ONLY",
  "user": {
    "firstName": "Test",
    "lastName": "Testington",
    "emailAddress": "testingtont@email.com",
    "address": "123 Fake St.",
    "city": "San Austonio",
    "state": "TX",
    "zipCode": "78031"
  },
  "paymentInformation": [
    {
      "amount": 500.01,
      "applyFundsTo": "ABC123456123",
      "type": "Checking", // Of type Checking, Savings, or Loan 
    }
  ],
  "PaymentAccountId": "4078"
}
```

2. ACH Only 

```js
Experience: 'NAO_ACH_ONLY'
```
#### Example Request Payload:
```js
{
  "experience": "NAO_ACH_ONLY",
  "user": {
    "firstName": "Test",
    "lastName": "Testington",
    "emailAddress": "testingtont@email.com",
    "address": "123 Fake St.",
    "city": "San Austonio",
    "state": "TX",
    "zipCode": "78031"
  },
  "paymentInformation": [
    {
      "amount": 500.01,
      "applyFundsTo": "ABC123456123",
      "type": "Checking", // Of type Checking, Savings, or Loan
    }
  ],	
  "PaymentAccountId": "4078" // Requires a capital letter (P)
}
```

3. ACH and Card 

```js
Experience: 'NAO_ACH_AND_CARD'
```
#### Example Request Payload:
```js
{
  "experience": "NAO_ACH_AND_CARD",
  "user": {
    "firstName": "Test",
    "lastName": "Testington",
    "emailAddress": "testingtont@email.com",
    "address": "123 Fake St.",
    "city": "San Austonio",
    "state": "TX",
    "zipCode": "78031"
  },
  "paymentInformation": [
    {
      "amount": 500.01,
      "applyFundsTo": "ABC123456123", 
      "type": "Checking", // Of type Checking, Savings, or Loan
    }
  ],
  "PaymentAccountId": "4078" // Requires a capital letter (P) 
} 
```
#### Example Request Payload, Multiple Payments:
```js
{
  "experience": "NAO_ACH_AND_CARD",
  "PaymentAccountId": "4078", // Requires a capital letter (P)
  "user": {
    "firstName": "Test",
    "lastName": "Testington",
    "emailAddress": "testingtont@email.com",
    "address": "123 Fake St.",
    "city": "San Austonio",
    "state": "TX",
    "zipCode": "78031"
  },
  "paymentInformation": [
    { 
      "amount": 500.01, 
      "applyFundsTo": "ABC123456123",  
      "type": "Checking", // Of type Checking, Savings, or Loan 
    },
    {
      "amount": 501.02,
      "applyFundsTo": "123456123ABC",
      "type": "Checking", // Of type Checking, Savings, or Loan
    },
  ]
}
```

## Success Payloads

```js
{
  "data": {
    "trackingNumber":"ARD2799940"
   },
  "iat":1632156834,
  "exp":1632157134
}

```

## Failure Payloads

1. User Exiting out of the flow.
```js
{
  "error": {
    "errorType": "USER_CANCELLED",
    "errorMessage": "The user cancelled the experience."
  },
  "iat":1632156834, 
  "exp":1632157134
}
```

2. User Inactive
```js
{ 
  "error": { 
    "errorType": "USER_TIMED_OUT",
    "errorMessage": "User timeout." 
  }, 
  "iat":1632156834,  
  "exp":1632157134 
}
```