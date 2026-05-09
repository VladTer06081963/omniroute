# OmniRoute

OmniRoute is a versatile routing service that allows you to manage and route requests to multiple providers seamlessly. It provides a user-friendly interface for adding and managing providers, and supports various programming languages for easy integration.

## Quick Start with Docker Compose

1. **Install Docker and Docker Compose** if you haven't already.
2. **Create a `.env` file** with the required environment variables (see below).
3. **Run the following command** to start the OmniRoute service:

```bash
docker-compose up -d
```

## Adding a Provider via UI

1. **Access the OmniRoute UI** at `http://localhost:20128`.
2. **Navigate to the Providers section**.
3. **Click on "Add Provider"** and fill in the required details.
4. **Save the provider** to start routing requests to it.

## Examples

### cURL

```bash
curl -X POST http://localhost:20128/api/route \
  -H "Content-Type: application/json" \
  -d '{"provider": "example", "data": {"key": "value"}}'
```

### Python

```python
import requests

url = "http://localhost:20128/api/route"
headers = {"Content-Type": "application/json"}
data = {"provider": "example", "data": {"key": "value"}}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

### Node.js

```javascript
const axios = require("axios");

const url = "http://localhost:20128/api/route";
const headers = {"Content-Type": "application/json"};
const data = {"provider": "example", "data": {"key": "value"}};

axios.post(url, data, { headers })
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
```

## Environment Variables

The following environment variables are required in your `.env` file:

```env
PORT=<YOUR_PORT>
NEXT_PUBLIC_BASE_URL=<YOUR_BASE_URL>
INITIAL_PASSWORD=<YOUR_INITIAL_PASSWORD>
ENABLE_REQUEST_LOGS=<YOUR_REQUEST_LOGS_SETTING>
GOOGLE_CLIENT_ID=<YOUR_GOOGLE_CLIENT_ID>
GOOGLE_CLIENT_SECRET=<YOUR_GOOGLE_CLIENT_SECRET>
```
