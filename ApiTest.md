### **Test Case Scenarios and API Validation**

The task involves testing a sequence of API calls (POST, GET, PUT, DELETE) from the **Video Game DB API** and performing validation on each request's response, including schema validation, status code checks, and other conditions.

---

### **1. Authentication API (POST request)**
- **Request URL**: `https://www.videogamedb.uk:443/api/authenticate`
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "username": "admin",
    "password": "admin"
  }
  ```
- **Expected Response**:
  - Status Code: `200 OK`
  - Response Body: Should return a `token` for authorization.
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImlhdCI6MTc0MjgwMjk5NiwiZXhwIjoxNzQyODA2NTk2fQ.pWenMkdyoh3QCRQWgTficw3tazc-c9A2xvf8nUZsJTQ"
    }
    ```

**Postman Test Script**:
```javascript
pm.test("Authenticate User and get Token", function () {
    pm.response.to.have.status(200);
    pm.response.to.have.jsonBody();
    var responseJson = pm.response.json();
    pm.environment.set("auth_token", responseJson.token);
});
```

---

### **2. Get List of Video Games (GET request)**

- **Request URL**: `https://www.videogamedb.uk:443/api/videogame`
- **Method**: `GET`
- **Headers**:
  - `Authorization`: `Bearer {{auth_token}}`
- **Expected Response**:
  - Status Code: `200 OK`
  - Response Body: A list of video games.
    ```json
    [
      {
        "id": 1,
        "name": "Super Mario",
        "category": "Platform",
        "rating": "Mature",
        "releaseDate": "2012-05-04",
        "reviewScore": 85
      },
      ...
    ]
    ```

**Postman Test Script**:
```javascript
pm.test("Get List of Video Games", function () {
    pm.response.to.have.status(200);
    var responseJson = pm.response.json();
    pm.expect(responseJson).to.be.an('array');
    pm.expect(responseJson[0]).to.have.property('id');
    pm.expect(responseJson[0]).to.have.property('name');
    pm.expect(responseJson[0]).to.have.property('releaseDate');
    pm.expect(responseJson[0]).to.have.property('reviewScore');
    pm.expect(responseJson[0]).to.have.property('category');
    pm.expect(responseJson[0]).to.have.property('rating');
});
```

---

### **3. Create a New Video Game (POST request)**

- **Request URL**: `https://www.videogamedb.uk:443/api/videogame`
- **Method**: `POST`
- **Headers**:
  - `Authorization`: `Bearer {{auth_token}}`
  - `Content-Type`: `application/json`
- **Request Body**:
  ```json
  {
    "category": "Platform",
    "name": "Mario",
    "rating": "Mature",
    "releaseDate": "2012-05-04",
    "reviewScore": 85
  }
  ```
- **Expected Response**:
  - Status Code: `200 OK`
  - Response Body: Should contain the created game's details along with an `id` field.
    ```json
    {
      "id": 123,
      "category": "Platform",
      "name": "Mario",
      "rating": "Mature",
      "releaseDate": "2012-05-04",
      "reviewScore": 85
    }
    ```

**Postman Test Script**:
```javascript
pm.test("Create a new video game", function () {
    pm.response.to.have.status(200); 
    pm.response.to.have.jsonBody(); 
    
});
```

---

### **4. Get Video Game by ID (GET request)**

- **Request URL**: `https://www.videogamedb.uk:443/api/videogame/{{game_id}}`
- **Method**: `GET`
- **Headers**:
  - `Authorization`: `Bearer {{auth_token}}`
- **Expected Response**:
  - Status Code: `200 OK`
  - Response Body: Should return the video game details for the specific ID.
    ```json
    {
      "id": {{game_id}},
      "category": "Platform",
      "name": "Mario",
      "rating": "Mature",
      "releaseDate": "2012-05-04",
      "reviewScore": 85
    }
    ```

**Postman Test Script**:
```javascript
    pm.test("Get video game by ID", function () {
    pm.response.to.have.status(200);
    pm.response.to.have.jsonBody();
    var responseId = pm.response.json().id;
    var expectedId = pm.environment.get("game_id");
    pm.expect(responseId).to.eql(Number(expectedId));  // Converting expectedId to a number
});
```

---

### **5. Update Video Game Details (PUT request)**

- **Request URL**: `https://www.videogamedb.uk:443/api/videogame/{{game_id}}`
- **Method**: `PUT`
- **Headers**:
  - `Authorization`: `Bearer {{auth_token}}`
  - `Content-Type`: `application/json`
- **Request Body**:
  ```json
  {
    "category": "Platform",
    "name": "Mario",
    "rating": "Mature",
    "releaseDate": "2012-05-04",
    "reviewScore": 90
  }
  ```
- **Expected Response**:
  - Status Code: `200 OK`
  - Response Body: The updated video game details.
    ```json
    {
      "id": {{game_id}},
      "category": "Platform",
      "name": "Mario",
      "rating": "Mature",
      "releaseDate": "2012-05-04",
      "reviewScore": 90
    }
    ```

**Postman Test Script**:
```javascript
pm.test("Update video game details", function () {
   pm.response.to.have.status(200);
   pm.response.to.have.jsonBody();
   var responseId = pm.response.json().id;
   var expectedId = pm.environment.get("game_id");
    pm.expect(responseId).to.eql(Number(expectedId));  // Converting expectedId to a number
});
```

---

### **6. Delete Video Game (DELETE request)**

- **Request URL**: `https://www.videogamedb.uk:443/api/videogame/{{game_id}}`
- **Method**: `DELETE`
- **Headers**:
  - `Authorization`: `Bearer {{auth_token}}`
- **Expected Response**:
  - Status Code: `200 OK`
  - Response Body: Should confirm the game was deleted.
    ```json
    {
      "message": "Video game deleted"
    }
    ```

**Postman Test Script**:
```javascript
    pm.test("Delete video game", function () {
    pm.response.to.have.status(200);
    pm.expect(pm.response.text()).to.equal("Video game deleted");
});
```

---

### **Test Scenario Combination**
Now that we have written individual test scripts for each API, the overall scenario will look like this:

1. **Authenticate** and get a `token`.
2. **Get list of games** using the token.
3. **Create a new game** and verify the ID.
4. **Get the newly created game** by ID and validate its details.
5. **Update the game** with new data and verify the updated details.
6. **Delete the game** and confirm that it is removed.

### **Postman Collection**

1. **Import the API documentation** into Postman using the provided Swagger link.
2. **Create a new Postman Collection** with these 6 requests, and set up the environment variables (`auth_token`, `game_id`).
3. **Export** the collection to share it with others.

To export the collection in Postman:
1. Go to **File** > **Export** and choose the format.
2. Save the `.json` file to share with others.
3. Go to **Enviroment** > **Export** and choose the format.
4. Save the `.json` file to share with others.

