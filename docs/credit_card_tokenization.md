
# Card Tokenization

### Required Info:


| Attribute   | Description |
| ----------- | ----------- |
| experience*  | Experience Type |
| accountId* | Swivel provided payment account id |
| firstName | User’s first name |
| lastName | User’s last name |
| address | User’s billing street address |
| city | User’s billing city |
| state | User’s state |
| zipCode | User’s zip code |

*required fields

### Initialization

```js
const Tokenization = new SWBC.Tokenization({
  "Mode": "MODE_SELECTOR",
  "IntegrationID": "INTEGRATION_ID",
  "AccountId": "4_DIGIT_ACCOUNT_ID",
  "Element": "YOUR_IFRAME_ELEMENT",
  "Experience": "EXPERIENCE_TYPE"
});
Tokenization.setResponseHandler(responseHandler);
```

### Starting User Flow

```js
Tokenization.startFlow(JWT);;
```

### Closing User Flow

```js
Tokenization.closeFlow();
```

### ACH Verification Experiences

1. Card Token

```js
Experience: 'CARD_TOKEN'
```
#### Example Request Payload:
```js
{
  "experience": "CARD_TOKEN",
  "user": { // user attribute and its fields are optional
    "firstName": "Test",
    "lastName": "Testington",
    "address": "123 Fake St.",
    "city": "San Austonio",
    "state": "TX",
    "zipCode": "78031"
  },
  "accountId": "4078"
}
```


## Success Response

```js
{
  "data": {
    "token": "85956175-54e6-4192-a991-d51d22fbe40e"
  },
  "iat":1632156834, 
  "exp":1632157134
}
```

## Failure Response

1. User exited experience.
```js
{
  error: {
    errorType: 'USER_CANCELLED',
    errorMessage: 'The user cancelled the iframe',
  }
}
```

2. User session expired due to inactivity
```js
{
  error: {
    errorType: 'USER_TIMED_OUT',
    errorMessage: 'User timeout.',
  }
}
```

3. Bad Request
```js
{ 
  "error": { 
    "errorType": "Bad Request",
    "errorMessage": "DECLINED"
  }, 
  "iat":1632156834,  
  "exp":1632157134 
}
```