
# ACH Verification

### Required Info:


| Attribute   | Description |
| ----------- | ----------- |
| experience  | Experience Type |
| ACHWebVerificationAccountId | Swivel provided payment account id |
| firstName | User’s first name |
| lastName | User’s last name |
| username | Email associated with the account | 

### Initialization

```js
const Verification = new SWBC.ACHWebVerification({ 
  "Mode": "MODE_SELECTOR", 
  "IntegrationID": "INTEGRATION_ID", 
  "ACHWebVerificationAccountId": "4_DIGIT_ACCOUNT_ID", 
  "Element": "YOUR_IFRAME_ELEMENT", 
  "Experience": "EXPERIENCE_TYPE" 
});
Verification.setACHWebVerificationHandler(responseHandler);
```

### Starting User Flow

```js
Verification.startACHWebVerification(signedRequest);
```

### Closing User Flow

```js
Verification.closeACHWebFlow();;
```

### ACH Verification Experiences

1. Verify One ACH Account

```js
Experience: 'VERIFY_ACH_ONE_ACCOUNT_ONLY'
```
#### Example Request Payload:
```js
{
  "experience": "VERIFY_ACH_ONE_ACCOUNT_ONLY",
  "user": { 
    "first_name": "first", 
    "last_name": "last", 
    "username": "firstlast@someemail.com"
  },
  "ACHWebVerificationAccountId": "4708"
}

```

2. Verify Multiple ACH Accounts

```js
Experience: 'VERIFY_ACH_MULTIPLE_ACCOUNTS'
```
#### Example Request Payload:
```js
{
  "experience": "VERIFY_ACH_MULTIPLE_ACCOUNTS",
  "user": { 
    "first_name": "first", 
    "last_name": "last", 
    "username": "firstlast@someemail.com"
  },
  "ACHWebVerificationAccountId": "4708"
}

```


## Success Payloads

```js
{
  "data": [
    {
      "data": {
        "accountNumber": "1000002222",
        "accountNumberLast4": "2222",
        "accountOwner": "JOHN DOE",
        "accountType": "savings", // checking or savings
        "routingNumber": "091000019",
        "nameOfInstitution": "FinBank",
        "institutionPhoneNumber": "" // if available
        "validationType": "web_validated",
        "vendor": "finicity"
      }
    },
    {
      "data": {
        "accountNumber": "1000001111",
        "accountNumberLast4": "1111",
        "accountOwner": "JOHN DOE",
        "accountType": "checking",
        "routingNumber": "091000019",
        "nameOfInstitution": "FinBank",
        "institutionPhoneNumber": "",
        "validationType": "web_validated", 
        "vendor": "finicity"
      }
    }
  ],
  "iat":1632156834,
  "exp":1632157134
}
```

## Failure Payloads

1. General Error
```js
{
  error: {
    errorType: 'ERROR',
    errorMessage: 'Runtime error was generated during Connect',
  }
}

```

2. Failure gathering required information.
```js
{
  error: {
    errorType: 'FAILED_DEPENDENCY',
    errorMessage: 'There were problems receiving all the required information for your request.',
  }
}
```

3. User's Financial Information was not found.
```js
{
  error: {
    errorType: 'INSTITUTION_NOT_FOUND',
    errorMessage: 'The institution was not found',
  }
}
```

4. User's Financial Institution is not supported.
```js
{
  error: {
    errorType: 'INSTITUTION_NOT_SUPPORTED',
    errorMessage: 'The selected institution is not supported',
  }
}
```

5. Failure on Setting up Experience.
```js
{
  error: {
    errorType: 'SETUP_ERROR',
    errorMessage: 'Failed to complete setup with information provided.',
  }
}
```

6. User exited experience.
```js
{
  error: {
    errorType: 'USER_CANCELLED',
    errorMessage: 'The user cancelled the iframe',
  }
}
```

7. User session expired due to inactivity
```js
{
  error: {
    errorType: 'USER_TIMED_OUT',
    errorMessage: 'User timeout.',
  }
}
```