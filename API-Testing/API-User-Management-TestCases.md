> 🔒 **Security & NDA Compliance Notice:**  
> All base URLs, authentication headers, API tokens, and specific client data in this document have been anonymized or replaced with generic placeholders (`example.com`, `<REDACTED_TOKEN>`) in compliance with Non-Disclosure Agreements (NDA) and data privacy standards.

---

# User Management API — Test Cases

###  TC-001: Verify successful creation of a new user profile with valid JSON payload
* **Preconditions:** API gateway is active; Client has valid authentication credentials (`Authorization: Bearer <REDACTED_TOKEN>`).
* **Steps:** Send a `POST` request to create a new user profile.
* **Test Data / Payload:**
  * **Base URL:** `https://api.example.com/v1`
  * **Endpoint:** `POST /users`
  * **Headers:** 
    * `Content-Type: application/json`
    * `Authorization: Bearer <REDACTED_BEARER_TOKEN>`
  * **Body:**
    ```json
    {
      "userId": 982104,
      "firstName": "Alex",
      "lastName": "surname",
      "email": "alex.surname@example.com",
      "role": "teacher",
      "accountStatus": "ACTIVE"
    }
    ```
* **Expected Result:**
  * **HTTP Status:** `200 OK` (or `201 Created`)
  * **Response Time:** `< 800ms`
  * **Response Body:** Returns full user profile object matching the request payload and confirms `userId: 982104`.

---

###  TC-002: Verify API response when requesting a non-existent User ID
* **Preconditions:** Client has valid authentication credentials.
* **Steps:** Send a `GET` request with a non-existent User ID.
* **Test Data / Payload:**
  * **Base URL:** `https://api.example.com/v1`
  * **Endpoint:** `GET /users/{userId}`
  * **Path Parameter:** `userId = 4324567`
  * **Headers:** `Authorization: Bearer <REDACTED_BEARER_TOKEN>`
* **Expected Result:**
  * **HTTP Status:** `404 Not Found`
  * **Response Body:**
    ```json
    {
      "code": "USER_NOT_FOUND",
      "message": "User profile with provided ID does not exist"
    }
    ```

---

### TC-003: Verify filtering users by Account Status (`ACTIVE`)
* **Preconditions:** Client has valid authentication credentials.
* **Step:** Send a `GET` request with a query parameter for active users.
* **Test Data / Payload:**
  * **Base URL:** `https://api.example.com/v1`
  * **Endpoint:** `GET /users/findByStatus`
  * **Query Param:** `status=ACTIVE`
  * **Headers:** `Authorization: Bearer <REDACTED_BEARER_TOKEN>`
* **Expected Result:**
  * **HTTP Status:** `200 OK`
  * **Response Body:** JSON Array `[]` containing user objects where every user has `"accountStatus": "ACTIVE"`.

---

### TC-004: Verify successful deletion of a user profile
* **Preconditions:** Target user ID (`982104`) exists in the database.
* **Steps:**
  1. Send a `DELETE` request for an existing user.
  2. Send a `GET` request for the deleted `userId` to verify removal.
* **Test Data / Payload:**
  1. **Endpoint:** `DELETE /users/{userId}`, **Path Parameter:** `userId = 4318`
  2. **Endpoint:** `GET /users/{userId}`, **Path Parameter:** `userId = 4318`
  * **Headers:** `Authorization: Bearer <REDACTED_BEARER_TOKEN>`
* **Expected Result:**
  1. **HTTP Status:** `200 OK` (or `204 No Content`). User profile is flagged as deleted/removed.
  2. **HTTP Status:** `404 Not Found`, confirming complete removal/deactivation from the backend.
