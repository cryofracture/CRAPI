# CRAPI - a "todo" Rust API

Welcome to CRAPI, a rust-developed application that connects to a PostgreSQL instance and provides RESTful HTTP responses asynchronously. Powered by [Actix-web](https://docs.rs/actix-web/4.3.1/actix_web/), [Diesel](https://docs.rs/diesel/2.1.0/diesel/), and [Rocket](https://docs.rs/rocket/0.5.0-rc.3/rocket/).

# Usage Instructions

### Docker container prep
To start the docker container:

`docker compose -f postgres.yml up`

After executing `cargo run`, you can interact with the API via cURL, Postman, or other API requests like a programming language:

## API Call Examples

### Create todo task
cURL:
```bash
curl -X POST -H "Content-Type: application/json" -d '{"title": "Buy bread", "description": "Buy 1 loaf of bread"}' http://localhost:8080/api/todos | jq
```

Python:
```python
import requests
import json

url = 'http://localhost:8080/api/todos'
headers = {'Content-Type': 'application/json'}
data = {
    'title': 'Buy bread',
    'description': 'Buy 1 loaf of bread'
}

response = requests.post(url, headers=headers, data=json.dumps(data))
response_json = response.json()

print(response_json)  # Output the response JSON
```

### Update existing task

cURL:
```bash
curl -s -X PUT -H "Content-Type: application/json" -d '{"title": "Buy milk", "description": "Buy 20 liters of milk"}' http://localhost:8080/api/todos/087c8867-91d6-4925-b07c-8aa05e811efc | jq
```

Python:
```python
import requests
import json

url = 'http://localhost:8080/api/todos/087c8867-91d6-4925-b07c-8aa05e811efc'
headers = {'Content-Type': 'application/json'}
data = {
    'title': 'Buy milk',
    'description': 'Buy 20 liters of milk'
}

response = requests.put(url, headers=headers, data=json.dumps(data))
response_json = response.json()

print(response_json)  # Output the response JSON
```

### Delete a todo task

cURL:
```bash
curl -s -X DELETE http://localhost:8080/api/todos/38498865-28b1-461f-a0cc-77b2bcc5e2ba | jq
```

Python:
```python
import requests

url = 'http://localhost:8080/api/todos/38498865-28b1-461f-a0cc-77b2bcc5e2ba'

try:
    response = requests.delete(url)
    response.raise_for_status()  # Raise an exception for non-2xx status codes

    print("Todo deleted successfully")
except requests.HTTPError as e:
    if response.status_code == 404:
        print("Todo not found")
    else:
        print("An error occurred during deletion:", str(e))
except requests.RequestException as e:
    print("Error occurred during the request:", str(e))
```

