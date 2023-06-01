# API Documentation for Users and Businesses

Welcome to the official documentation of Trailingcrypto API. This guide provides information on how to use the API endpoints for user-related operations.

## Table of Contents
- [Check Server Status](#check-server-status)
- [Sign Up](#sign-up)
- [Request Reset Password](#request-reset-password)
- [Reset Password](#reset-password)
- [Login](#login)
- [User Profile](#user-profile)
- [Connect Exchange](#connect-exchange)
- [Delete Exchange](#delete-exchange)
- [Edit Exchange Connection](#edit-exchange-connection)
- [Get Exchange Connection Info](#get-exchange-connection-info)

## Important Note
Always include the following key-value pair in the header to receive the response in JSON format:

```
x-requested-with: XMLHttpRequest
Content-Type: application/x-www-form-urlencoded
```

## Check Server Status

**Request:**

```http
GET /api/ping
```

**Response:**

```javascript  
{
   "err": false,
   "msg": "pong",
   "time": 1569750557563
}
```

## Sign Up
- Note: This requires **Business Apikey**. Contact us for more info support@trailingcrypto.com
- Method: POST
- Endpoint: `/auth/email/register`
- Header:
  - Authorization: Bearer business_api_key (constant)
- Body:
  ```json
  {
    "name": "Vixxxxx",
    "email": "vixxxxxx@yopmail.com",
    "password": "1xxxxx5",
    "bypass_captcha_key": "kasjdhXXXXX-XXXX-XXaiu" (constant)
  }
  ```
- Response:
  - Success:
    ```json
    {
      "error": false,
      "msg": "Verification mail sent"
    }
    ```
  - Failure:
    ```json
    {
      "error": true,
      "msg": "Email already Exist"
    }
    ```

## Request Reset Password
- Method: GET
- Endpoint: `/auth/email/reset`
- Parameters:
  - token
  - email: viXXXxxxxxxxxx@gmail.com
  - password: password
  - bypass_captcha_key: kasjdhXXXXX-XXXX-XXaiu (constant)
- Response:
  - Success:
    ```json
    {
      "error": false,
      "msg": "Password reset mail sent"
    }
    ```
  - Failure:
    ```json
    {
      "error": true,
      "msg": "Email is not registered."
    }
    ```

## Reset Password
- Method: POST
- Endpoint: `/auth/email/reset`
- Parameters:
  - token: eyJhbGcixxxxxxxxsIn....token sent in the email URL
  - email: user@email.com
  - password: xxxxx
  - bypass_captcha_key: kasjdhXXXXX-XXXX-XXaiu (constant)
- Response:
  - Success:
    ```json
    {
      "error": false,
      "msg": "Password Reset Successful!"
    }
    ```

## Login
- Method: POST
- Endpoint: `/auth/email/login`
- Parameters:
  - email: vixxxxxxxx@gmail.com
  - password: 12345
  - bypass_captcha_key: kasjdhXXXXX-XXXX-XXaiu (constant)
- Response:
  ```json
  {
    "id": "",
    "_id": "",
    "name": "",
    "email": "",
    "identifier": "",
    "provider": "",
    "type": "",
    "referred": "",
    "apikey": "UserXXXApiXXXkey"
  }
  ```
- **Important Note**:
  Save this API key in local storage or somewhere and pass this API key for all other requests (UserXXXApiXXXkey) in the header as:
  ```
  Authorization: Bearer UserXXXApiXXXkey
  ```
  This will authenticate all other API requests except user register.
- Failure:
  Redirects 302: domain.com/#loginerror



## User Profile
- Method: GET
- Endpoint: `/api/user/profile`
- Parameters: [None]
- Response:
  ```json
  {
    "id": "5f1xxxxxxxxxxxx607de", // user_id
    "_id": "5f1xxxxxxxxxxxx607de", // user_id
    "name": "User name ", // user_name
    "email": "useremailksd@gmail.com", // user_email
    "provider": "email",
    "type": 1, // payment_type
    "referred": 0,
    "apikey": "e2bb2b3d-08e0-xxxx-xxxx-x-28f2cc7efee", // user_api_key
    "telegram": { // telegram object (for telegram notifications)
      "verified": true,
      "enabled": false,
      "token": "b35xxxxxxxxxxxxxxx7292",
      "userId": "12xxxxxxxxxxxxx08"
    },
    "exchange_list": ["coinex", "binance_BinanceAPIkey"] // connected exchanges
  }
  ```
- Possible Values:
   - provider: email, google, facebook
   - type: TRIAL:1, FREE:2, PAID:3, TELEGRAM_ADMIN:4, BUSINESS:5
   - referred: Count of users signed-up using current user referral link

## Connect Exchange
- Method: POST
- Endpoint: `/api/user/saveapikey`
- Parameters:
  - exchange: binance/bittrex/kucoin/etc
  - key: asdfasdf
  - uid:
  - secret: asdfasdf
  - password: asdfasdf
  - label: asdfasdf
- Response:
  - Success:
    ```json
    {
      "status": true
    }
    ```
  - Failure:
    ```json
    {
      "status": false,
      "msg": "Invalid Exchange"
    }
    ```

## Delete Exchange
- Method: DELETE
- Endpoint: `/api/user/apikey`
- Parameters:
  - exchange: okex3
  - label: asdfasdf
- Response:
  ```json
  {
    "success": true,
    "msg": "okex3_asdfasdf API Key deleted successfully"
  }
  ```

## Edit Exchange Connection
- Method: POST
- Endpoint: `/api/user/saveapikey`
- Parameters:
  - exchange: coinbasepro
  - key: asfasdfxxxxsdfgdfg
  - uid:
  - secret: ksjxxxx-xxxx-x-x-xkasjdxx80-xjkjxxxxxx-xxx
  - password: sdfgsdfg
  - label: Defaultaf
- Response:
  ```json
  {
    "status": true
  }
  ```

## Get Exchange Connection Info
- Method: GET
- Endpoint: `/api/user/apikey`
- Parameters:
  - exchange: coinbasepro
  - label: Defaultaf
- Response:
  ```json
  {
    "apiKey": "asfasdfxx-xxx-xx-sdfgdfg",
    "encrypted": true,
    "label": "Defaultaf",
    "password": "sdfxx-xxx-xxx-xdfg",
    "secret": "YouCant-xxx-xxxx-SeeMe",
    "type": "",
    "uid": ""
  }
  ```
