## Automated-Testing-of-Rest-Booking-API-with-Newman-Report

<p>This project showcases API testing using Postman, offering a comprehensive collection of tests designed to validate and verify the functionality of various API endpoints. The tests aim to ensure the reliability, correctness, and performance of the API by covering a wide range of scenarios and use cases.</p>

## Features

- Tests for `GET`, `POST`, `PUT`, `DELETE` requests
- Collection of tests covering various API endpoints
- Environment setup for seamless switching between environments
- Pre-request scripts for data initialization
- Test scripts for assertions and validations

## API Documentation

- [https://documenter.getpostman.com/view/13082503/2sA2xmUAJ1].

## Technology Used

- **Postman**
- **Newman**

## Prerequisites

1. **Node.js**  
2. **Newman**
3. **Newman HTML Report Library**


## Installation

### 1. Postman
If you haven't already, [download and install Postman](https://www.postman.com/downloads/).

### 2. Clone the Repository
Clone this repository to your local machine:  
```bash
git clone https://github.com/jeba-tahseen2712/Automated-Testing-of-Rest-Booking-API-with-Newman-Report.git
```

### 3. Import the Postman Collection
-Open Postman.
-Click on the Import button.
-Select the Postman collection file from the cloned repository.
### 4. Import the Postman Environment
-In Postman, click on the gear icon in the top-right corner.
-Select **Import** and choose the Postman environment file from the cloned repository.

### 5. Newman and Report Installation:<br>
-Install Newman globally & Newman Html Report Install Command:
```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```
## Usage

### 1. Select Environment
- In **Postman**, choose the appropriate environment (e.g., Development, Production) from the dropdown menu in the top-right corner.

### 2. Run Collection
1. Select the imported collection from the **Collections** sidebar.
2. Click on the **Runner** button to open the Collection Runner.
3. Choose the desired environment.
4. Click **Start Test** to execute the collection.

### 3. View Results
- Once the tests are complete, view the results in the **Runner** tab.
- Detailed test results can be reviewed for each request.

## Testing

### Test Case Scenarios

#### 1. Create New Booking

- **Request URL:**  
  `https://restful-booker.herokuapp.com/booking/`

- **Request Method:**  
  `POST`

- **Pre-request Script:**  
  
```bash
var firstname=pm.variables.replaceIn("{{$randomFirstName}}")
pm.environment.set("firstname", firstname)

var lastname=pm.variables.replaceIn("{{$randomLastName}}")
pm.environment.set("lastname", lastname)

var totalprice= pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("totalprice",totalprice)

var depositpaid= pm.variables.replaceIn("{{ $randomBoolean}}")
pm.environment.set("depositpaid", Boolean(depositpaid))

const moment= require("moment")
const today = moment()
pm.environment.set("checkin", today.format("YYYY-MM-DD"))
pm.environment.set("checkout", today.add(4,'M').format("YYYY-MM-DD"))

var additionalneeds =pm.variables.replaceIn("{{$randomProduct}}")
pm.environment.set("additionalneeds",additionalneeds)
```

- **Post-request Script:**  
  
```bash
var jsonData= pm.response.json()
pm.environment.set("ID", jsonData.bookingid)

pm.test("Successful POST request", function () {
    pm.expect(pm.response.code).to.be.oneOf([200, 202]);
});

pm.test("Response time is less than two thousands ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});
```

- **Request Body:**  
  
```bash
  {
     "firstname" : "{{firstName}}",
     "lastname" : "{{lastName}}",
     "totalprice" : {{totalPrice}},
     "depositpaid" : {{depositPaid}},
     "bookingdates" : {
   	  "checkin" : "{{checkin}}",
   	  "checkout" : "{{checkout}}"
     },
     "additionalneeds" : "{{additionalNeeds}}"
 }
```

- **Response Body:**  
  
```bash
{
    "bookingid": 3113,
    "booking": {
        "firstname": "Donato",
        "lastname": "O'Conner",
        "totalprice": 852,
        "depositpaid": true,
        "bookingdates": {
            "checkin": "2024-11-26",
            "checkout": "2025-03-26"
        },
        "additionalneeds": "Shoes"
    }
}
```
#### 2. Get Booking Details By ID

- **Request URL:**  
  `https://restful-booker.herokuapp.com/booking/{bookingid}`

- **Request Method:**  
  `GET`

  - **Post-request Script:**  
  
```bash
var jsonData= pm.response.json();

pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});



pm.test("First name validation", function () {
    pm.expect(jsonData.firstname).to.be.a('string');
});

pm.test("First name validation", function () {
    pm.expect(jsonData.firstname).to.eql(pm.environment.get("firstname"))
});


pm.test("Last name varification", function () {
     pm.expect(jsonData.lastname).to.exist.and.to.be.a('string');
});

pm.test("Last name varification", function () {
     pm.expect(jsonData.lastname).to.eql(pm.environment.get("lastname"))
});

pm.test("Total price validation", function () {
     pm.expect(jsonData.totalprice).to.be.a('number');
});

pm.test("Total price validation", function(){
    pm.expect(jsonData.totalprice).to.eql(Number(pm.environment.get("totalprice")));
});

pm.test("verify deppositpaid", function () {
     pm.expect(jsonData.depositpaid).to.be.a('boolean');
});

pm.test("Depositpaid varification", function(){
    pm.expect(jsonData.depositpaid).to.eql(pm.environment.get("depositpaid"))
});


pm.test("Checkin Valiation", function () {
     pm.expect(pm.response.json()).to.be.an('object');
     pm.expect(pm.response.json().bookingdates.checkin).to.exist.and.to.be.a('string');
});

pm.test("Checkin Validation",function(){
    pm.expect(jsonData.bookingdates.checkin).to.eql(pm.environment.get("checkin"))
});

pm.test("Checkout Validation",function(){
    pm.expect(jsonData.bookingdates.checkout).to.eql(pm.environment.get("checkout"))
})

pm. test("Verify additionalneeds",function(){
    pm.expect(jsonData.additionalneeds).to.eql(pm.environment.get("additionalneeds"))
});
```

- **Response Body:**  
  ```bash
  {
    "firstname": "Evelyn",
    "lastname": "Will",
    "totalprice": 59,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-11-26",
        "checkout": "2025-03-26"
    },
    "additionalneeds": "Car"}
  
  ```

### 3. Create Token

- **Request URL:**  
  `https://restful-booker.herokuapp.com/auth`

- **Request Method:**  
  `POST`

- **Pre-request Script:**  
  None

- **Request Body:**  

```bash
{
   "username": "admin",
   "password": "password123"
}
```

- **Response Body:**
```bash
{
    "token": "955eb1696e2340a"
}
```
### 4. Update the Booking Details

- **Request URL:**  
  `https://restful-booker.herokuapp.com/booking/{bookingid}`

- **Request Method:**  
  `PUT`

- **Pre-request Script:**

```bash
var firstname=pm.variables.replaceIn("{{$randomFirstName}}")
pm.environment.set("Ufirstname", firstname)

var lastname=pm.variables.replaceIn("{{$randomLastName}}")
pm.environment.set("Ulastname", lastname)

var totalprice= pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("Utotalprice",totalprice)

var depositpaid= pm.variables.replaceIn("{{ $randomBoolean}}")
pm.environment.set("Udepositpaid", Boolean(depositpaid))

const moment= require("moment")
const today = moment()
pm.environment.set("Ucheckin", today.format("YYYY-MM-DD"))
pm.environment.set("Ucheckout", today.add(4,'M').format("YYYY-MM-DD"))

var additionalneeds =pm.variables.replaceIn("{{$randomProduct}}")
pm.environment.set("Uadditionalneeds",additionalneeds)
```

- **Request Body:**
```bash
{
	"firstname" : "{{Ufirstname}}",
	"lastname" : "{{Ulastname}}",
	"totalprice" :"{{Utotalprice}}",
	"depositpaid" : "{{Udepositpaid}}",
	"bookingdates" : {
    	"checkin" : "{{Ucheckin}}",
    	"checkout" : "{{Ucheckout}}"
	},
	"additionalneeds" : "{{Uadditionalneeds}}"
}
```

- **Response Body:**

```bash
{
    "firstname": "Jesus",
    "lastname": "Heaney",
    "totalprice": 87,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-11-26",
        "checkout": "2025-03-26"
    },
    "additionalneeds": "Salad"
}
```

### 5. Delete Booking Record

- **Request URL:**  
  `https://restful-booker.herokuapp.com/booking/{bookingid}`

- **Request Method:**  
  `DELETE`

- **Response Body:**  
  None

### Run Command:  

- **Run Command for Report:**  
newman run Batch28-2024.postman_collection.json -e batch28-2024.postman_environment.json


### Report:

![image](https://github.com/user-attachments/assets/804cec79-fe46-448e-ba16-a7163af19cc3)
![image](https://github.com/user-attachments/assets/ef025ce1-7003-4ed8-9dbb-3a93683fd5e0)
![image](https://github.com/user-attachments/assets/bbc69e2a-fcfe-4831-8c4a-ab1c4a27dfc8)
![image](https://github.com/user-attachments/assets/bc469b99-3ede-4cb1-a821-b5274f42214f)
![image](https://github.com/user-attachments/assets/b7acdb28-8bff-42cd-83ab-44d04a2b4e7e)



