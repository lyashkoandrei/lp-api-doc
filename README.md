# Skip Trace API

The **Skip Trace API** allows you to create skip trace tasks and retrieve their status.  
All requests must include a valid **JWT token** in the `Authorization` header.

**Base URL:**  
```
https://landportal.com/wp-json/lp-rest-api/v1
```

---

## 📑 Table of Contents
- [Authentication](#authentication)
- [Create Task](#1-create-task)
  - [File Input](#option-1-file-via-url)
  - [JSON Input](#option-2-inline-json-data)
  - [Examples (cURL / Python / JS)](#examples)
- [Get Task Status](#2-get-task-status)
  - [Examples (cURL / Python / JS)](#examples-1)
- [Errors](#errors)
- [File Handling](#file-handling)
- [Token System](#token-system)

---

## Authentication

All requests must include a **JWT token**:

```
Authorization: Bearer <JWT_TOKEN>
```

---

## 1. Create Task

### Endpoint
```
POST /skip-trace
```

### Headers
```
Content-Type: application/json
Authorization: Bearer <JWT_TOKEN>
```

### Required Conditions
- Active subscription  
- Sufficient tokens  
- Either `input_file` or `json_data` must be provided  
- When using `input_file`, a `fields` mapping must be included  

---

### Option 1: File via URL
```json
{
  "method": "create",
  "input_file": "https://example.com/data.csv",
  "has_header": true,
  "fields": {
    "last_name": "last_name",
    "first_name": "first_name",
    "mailing_address": "mailing_address",
    "mailing_city": "mailing_city",
    "mailing_state": "mailing_state",
    "mailing_zip": "mailing_zip",
    "property_address": "property_address",
    "property_city": "property_city",
    "property_state": "property_state",
    "property_zip": "property_zip"
  },
  "emails_trace": false
}
```

### Option 2: Inline JSON Data
```json
{
  "method": "create",
  "json_data": [
    {
      "last_name": "Smith",
      "first_name": "John",
      "mailing_address": "123 Main St",
      "mailing_city": "New York",
      "mailing_state": "NY",
      "mailing_zip": "10001",
      "property_address": "456 Oak Ave",
      "property_city": "Brooklyn",
      "property_state": "NY",
      "property_zip": "11201"
    }
  ],
  "emails_trace": false
}
```

---

### Success Response
```json
{
  "success": true,
  "message": "Skip trace created successfully",
  "data": {
    "data_length": 100,
    "input_file": "https://s3.amazonaws.com/bucket/skip-trace-files/uuid_userid_timestamp.csv",
    "task_id": 12345,
    "tokens_left": 950
  }
}
```

---

### Examples

#### cURL
```bash
curl -X POST "https://landportal.com/wp-json/lp-rest-api/v1/skip-trace" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "method": "create",
    "json_data": [{"last_name":"Smith","first_name":"John"}]
  }'
```

#### Python (requests)
```python
import requests

url = "https://landportal.com/wp-json/lp-rest-api/v1/skip-trace"
headers = {"Authorization": "Bearer <JWT_TOKEN>"}
payload = {
    "method": "create",
    "json_data": [{"last_name": "Smith", "first_name": "John"}]
}

response = requests.post(url, json=payload, headers=headers)
print(response.json())
```

#### JavaScript (fetch)
```javascript
const response = await fetch("https://landportal.com/wp-json/lp-rest-api/v1/skip-trace", {
  method: "POST",
  headers: {
    "Authorization": "Bearer <JWT_TOKEN>",
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    method: "create",
    json_data: [{ last_name: "Smith", first_name: "John" }]
  })
});

const data = await response.json();
console.log(data);
```

### PHP
```php
<?php
$token = "<JWT_TOKEN>";
$url = "https://landportal.com/wp-json/lp-rest-api/v1/skip-trace";

$data = [
    "method" => "create",
    "json_data" => [
        [
            "last_name" => "Smith",
            "first_name" => "John",
            "mailing_address" => "123 Main St",
            "mailing_city" => "New York",
            "mailing_state" => "NY",
            "mailing_zip" => "10001",
            "property_address" => "456 Oak Ave",
            "property_city" => "Brooklyn",
            "property_state" => "NY",
            "property_zip" => "11201"
        ]
    ],
    "emails_trace" => false
];

$options = [
    "http" => [
        "header" => "Content-Type: application/json\r\nAuthorization: Bearer $token\r\n",
        "method" => "POST",
        "content" => json_encode($data)
    ]
];

$context = stream_context_create($options);
$result = file_get_contents($url, false, $context);

if ($result === FALSE) {
    die("Error while calling API");
}

echo $result;
```

---

## 2. Get Task Status

### Endpoint
```
POST /skip-trace
```

### Input Example
```json
{
  "method": "get",
  "task_id": 12345
}
```

---

### Success Response
```json
{
  "success": true,
  "message": "Skip trace data",
  "data": {
    "task_id": 12345,
    "task_status": "completed",
    "total_rows": 100,
    "success_rows": 95,
    "output_file_csv": "https://api.landportal.com/files/uuid_file.csv"
  }
}
```

---

### Examples

#### cURL
```bash
curl -X POST "https://landportal.com/wp-json/lp-rest-api/v1/skip-trace" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"method":"get","task_id":12345}'
```

#### Python
```python
payload = {"method": "get", "task_id": 12345}
response = requests.post(url, json=payload, headers=headers)
print(response.json())
```

#### JavaScript
```javascript
const res = await fetch(url, {
  method: "POST",
  headers,
  body: JSON.stringify({ method: "get", task_id: 12345 })
});
console.log(await res.json());
```

---

## Errors

### 401 Unauthorized
```json
{ "success": false, "message": "User not authenticated" }
```

### 400 Validation
```json
{ "success": false, "message": "Input file or JSON data is required" }
```

### 403 Forbidden
```json
{ "success": false, "message": "You are not authorized to get this skip trace" }
```

### 500 Server Error
```json
{ "success": false, "message": "Internal server error occurred" }
```

---

## File Handling
- **Supported format**: CSV  
- **Encoding**: UTF-8  
- **Max size**: Limited by available tokens  
- **URLs supported**: Direct links, Google Drive links  

---

## Token System
- **1 row = 1 token**  
- Tokens are deducted on task creation  
- Failed rows refund tokens  
