# 📡 Volley & PHP Communication Guide

এই ডকুমেন্টে Android **Volley Library** ব্যবহার করে PHP সার্ভারের সাথে তিনভাবে ডাটা আদান–প্রদান দেখানো হয়েছে:

1. **String Request (POST Form Data)**
2. **JSON Object Request**
3. **JSON Array Request**

---

# ✅ 1. String Request (Form POST)

## 📱 Android (Java – Volley)

```java
private void requestServer() {

    String url = "https://bongotech.xyz/juba.php";

    StringRequest stringRequest = new StringRequest(
            Request.Method.POST,
            url,
            response -> {
                textView.setText(response);
            },
            error -> {
                textView.setText("Error: " + error.getMessage());
            }
    ) {
        @Override
        protected Map<String, String> getParams() {

            Map<String, String> myMap = new HashMap<>();
            myMap.put("pass", "113355");
            myMap.put("email", "juba@gmail.com");

            return myMap;
        }
    };

    RequestQueue requestQueue = Volley.newRequestQueue(this);
    requestQueue.add(stringRequest);
}
```

---

## 🌐 PHP (Receive String Request)

```php
<?php

$pass  = $_POST['pass'] ?? '';
$email = $_POST['email'] ?? '';

if ($pass === '113355') {
    echo "Your password is correct. Your email is: $email";
} else {
    exit("Invalid password");
}
```

---

# ✅ 2. JSON Object Request

## 📱 Android (Java)

```java
private void objectRequest() {

    String url = "https://bongotech.xyz/apps/juba.php";

    JSONObject jsonObject = new JSONObject();
    try {
        jsonObject.put("pass", "113355");
        jsonObject.put("email", "jubayer@gmail.com");
    } catch (JSONException e) {
        e.printStackTrace();
    }

    JsonObjectRequest objectRequest = new JsonObjectRequest(
            Request.Method.POST,
            url,
            jsonObject,
            response -> {
                textView.setText(response.toString());
            },
            error -> {
                textView.setText("Error: " + error.getMessage());
            }
    );

    RequestQueue requestQueue = Volley.newRequestQueue(this);
    requestQueue.add(objectRequest);
}
```

---

## 🌐 PHP (Receive JSON Object)

```php
<?php

$json = file_get_contents("php://input");
$data = json_decode($json, true);

$pass  = $data['pass']  ?? '';
$email = $data['email'] ?? '';

$response = array();

if ($pass === '113355') {

    $response['type'] = "Success";
    $response['output'] = "Your password is correct. Your email is: $email";

} else {

    $response['type'] = "Error";
    $response['output'] = "Invalid password";
}

echo json_encode($response);
```

---

# ✅ 3. JSON Array Request

## 📱 Android (Java)

```java
private void jsonRequest() {

    String url = "https://bongotech.xyz/apps/juba.php";

    JSONArray jsonArray = new JSONArray();
    JSONObject jsonObject = new JSONObject();

    try {
        jsonObject.put("pass", "113355");
        jsonObject.put("email", "jubayer@gmail.com");
        jsonArray.put(jsonObject);
    } catch (JSONException e) {
        e.printStackTrace();
    }

    JsonArrayRequest arrayRequest = new JsonArrayRequest(
            Request.Method.POST,
            url,
            jsonArray,
            response -> {
                textView.setText(response.toString());
            },
            error -> {
                textView.setText("Error: " + error.getMessage());
            }
    );

    RequestQueue requestQueue = Volley.newRequestQueue(this);
    requestQueue.add(arrayRequest);
}
```

---

## 🌐 PHP (Receive JSON Array)

```php
<?php

$json = file_get_contents("php://input");
$jsonArray = json_decode($json, true);

$data = $jsonArray[0];

$pass  = $data['pass'] ?? '';
$email = $data['email'] ?? '';

$result = array();

if ($pass === '113355') {

    $temp = array();
    $temp['type'] = "Success";
    $temp['output'] = "Your password is correct. Your email is: $email";

    array_push($result, $temp);
}

echo json_encode($result);
```
