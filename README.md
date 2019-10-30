# api-client-python
The library offers easy access to Moneymour APIs

## Installation

A PyPI package is available

```bash
$ pip install moneymour-api-client
```

## Usage

```python

from moneymour import environments
from moneymour.api_client import ApiClient

# Get the following information from https://merchant.sandbox.moneymour.com
# For production: https://merchant.moneymour.com
PRIVATE_KEY = '<YOUR_PRIVATE_KEY>'
MERCHANT_ID = '<YOUR_MERCHANT_ID>'
MERCHANT_SECRET = '<YOUR_MERCHANT_SECRET>'

# Build the client
client = ApiClient(MERCHANT_ID, MERCHANT_SECRET, environments.ENVIRONMENT_SANDBOX)

# Request payload
payload = {
    'orderId': '123456',  # the order id in your system
    'amount': '1080',  # must be >= 300 and <= 2000
    'email': 'customer@merchant.com',
    'phoneNumber': '+393334444555',  # must include +39
    'products': [  # the list of products in the cart
      {
        'name': 'iPhone 7',
        'type': 'Electronics',
        'price': '500',
        'quantity': 2,
        'discount': 0
      },
      {
        'name': 'MacBook Pro Charger',
        'type': 'Electronics',
        'price': '80',
        'quantity': 1,
        'discount': 0
      }
    ]
}

# Perform the request
response = client.request(PRIVATE_KEY, payload)

# Output response
print(response)

'''
{
    "status": "accepted",
    "amount": 1080,
    "phoneNumber": "+393334444555",
    "orderId": "123456",
    "products": [
        {
            "name": "iPhone 7",
            "type": "Electronics",
            "price": "500",
            "price": "2",
            "discount": 0
        },
        {
            "name": "MacBook Pro Charger",
            "type": "Electronics",
            "price": "80",
            "price": "1",
            "discount": 0
        }
    ]
}
'''
```

## Gotchas

Moneymour APIs allow only one pending request at a time. If you get a **403 error** having message **Duplicated request** please cancel the current pending request using the Moneymour app for iOS or Android.

## Verify signature in your webhook

```python

from moneymour import environments
from moneymour.crypto_utils import Signature

Signature.verify(
    signature,  # http request header "Signature"
    expires_at,  # http request header "Expires-at"
    body,  # http request body
    environment=environments.ENVIRONMENT_SANDBOX
)  # return true or false
```
