# Prescreening Technical Questions

During the review process, we will pay attention to the code's neatness, proper variable naming, and solution correctness. Syntax accuracy will be considered to a lesser extent. If you have any comments or additional details, please include them as well.

## Chapter 1 - SQL

We have the following tables:

- `books` table with the fields: book_id, book_title, author_id
- `authors` table with the fields: author_id, author_name

#### 1. Write an SQL query that returns the names of authors who have written more than 2 books. Author names are not unique.

Answer:

SELECT a.author_id
FROM authors a
JOIN books b ON a.author_id = b.author_id
GROUP BY a.author_id, a.author_name
HAVING COUNT (b.book_id) > 2

#### 2. In few words what changes or additions in schema do we need to make to allow a book to have multiple authors?

Answer:

We need to introduce a join table that establishes a many-to-many relationship between books and authors (it is one-to-many now).

## Chapter 2 - Another SQL

We have 2 tables:

1. payments_principal – paid principal debt
2. payments_interest – paid interest debt

Please write SQL query which will show total paid amount of principal and interest.
Amounts should be calculated only for agreements which paid both principal and interest (at least once).

```sql
  drop table if exists #payments_principal
  create table #payments_principal (
    payment_dt date,
    agr_id smallint,
    payment_sum int
  )

  drop table if exists #payments_interest
  create table #payments_interest (
    payment_dt date,
    agr_id smallint,
    payment_sum int
  )

  insert into #payments_principal values ('2019-05-11', 31, 8281)
  insert into #payments_principal values ('2019-05-12', 7, 4622)
  insert into #payments_principal values ('2019-05-13', 5, 7686)
  insert into #payments_principal values ('2019-07-01', 1, 9917)
  insert into #payments_principal values ('2019-07-23', 1, 6534)
  insert into #payments_principal values ('2019-08-20', 64, 4336)
  insert into #payments_principal values ('2019-08-24', 3, 7464)
  insert into #payments_principal values ('2019-08-25', 9, 8505)
  insert into #payments_principal values ('2019-08-27', 1, 9857)
  insert into #payments_principal values ('2019-07-07', 7, 6294)
  insert into #payments_principal values ('2019-07-17', 7, 3182)
  insert into #payments_principal values ('2019-08-28', 4, 9708)
  insert into #payments_principal values ('2019-08-29', 4, 8632)
  insert into #payments_principal values ('2019-08-30', 3, 8303)
  insert into #payments_principal values ('2019-09-01', 7, 3141)
  insert into #payments_principal values ('2019-08-25', 1, 9139)
  insert into #payments_principal values ('2019-08-25', 2, 7624)
  insert into #payments_principal values ('2019-09-01', 7, 3793)
  insert into #payments_principal values ('2019-09-01', 3, 3260)
  insert into #payments_principal values ('2019-08-21', 5, 9002)
  insert into #payments_principal values ('2019-08-22', 2, 5500)
  insert into #payments_principal values ('2019-05-12', 7, 4622)
  insert into #payments_principal values ('2019-08-23', 2, 3980)
  insert into #payments_principal values ('2019-08-29', 2, 5849)


  insert into #payments_interest values ('2019-05-11', 31, 98)
  insert into #payments_interest values ('2019-05-12', 7, 90)
  insert into #payments_interest values ('2019-05-13', 5, 39)
  insert into #payments_interest values ('2019-07-01', 1, 82)
  insert into #payments_interest values ('2019-07-23', 1, 59)
  insert into #payments_interest values ('2019-08-20', 50, 96)
  insert into #payments_interest values ('2019-08-24', 3, 1)
  insert into #payments_interest values ('2019-08-25', 9, 22)
  insert into #payments_interest values ('2019-08-27', 1, 95)
  insert into #payments_interest values ('2019-07-07', 7, 79)
  insert into #payments_interest values ('2019-07-17', 7, 72)
  insert into #payments_interest values ('2019-08-28', 4, 61)
  insert into #payments_interest values ('2019-08-29', 4, 49)
  insert into #payments_interest values ('2019-08-30', 3, 78)
  insert into #payments_interest values ('2019-09-01', 7, 29)
  insert into #payments_interest values ('2019-08-25', 1, 88)
  insert into #payments_interest values ('2019-08-25', 2, 77)
  insert into #payments_interest values ('2019-09-01', 6, 6)
  insert into #payments_interest values ('2019-09-01', 3, 18)
  insert into #payments_interest values ('2019-08-21', 5, 15)
  insert into #payments_interest values ('2019-08-22', 2, 28)
  insert into #payments_interest values ('2019-08-23', 2, 23)
  insert into #payments_interest values ('2019-08-29', 2, 84)
```

Your answer should include SQL query and its results with output agreement id, sum of principal paid amount, sum of interest paid amount.

Answer:

```sql

SELECT
  p.agr_id,
  SUM(p.payment_sum) AS total_principal_paid,
  SUM(i.payment_sum) AS total_interest_paid,
  SUM(p.payment_sum) + SUM(i.payment_sum) AS total_paid
FROM
  #payments_principal p
JOIN
  #payments_interest i ON p.agr_id = i.agr_id
GROUP BY
  p.agr_id
HAVING
  COUNT(DISTINCT p.payment_dt) > 0
  AND COUNT(DISTINCT i.payment_dt) > 0;

Results:
agr_id  total_principal_paid  total_interest_paid  total_paid
1   	      141788	              1296	              143084
2	          91812	                848	                 92660
3         	57081	                291	                 57372
4         	36680	                220	                 36900
5	          33376	                108	                 33484
7	          102616	              1620	               104236
9	          8505	                22	                 8527

```

## Chapter 3 - Unit Tests

Please add test cases for the divide function. For simplicity, let's assume we are working only with integer numbers.

```java
int divide(int a, int b) {
  if (b == 0) {
    throw RuntimeException("divided by 0")
  }

  return a / b;
}
```

Answer:

- divide(1, 1) // will return 1
- divide(0, 1) // will return 0
- divide(4, -2) // will return -2
- divide(-4, 2 ) // will return -2
- divide(-4, -2 ) // will return 2
- divide(3, 1 ) // will return 3
- divide(3, 3 ) // will return 1
- divide(7, 2 ) // will return 3
- divide(3, 0 ) // throws an exception

## Chapter 4 - Integration Tests

Assume you have an API with the following endpoints:

- `POST /cart/add`: Add items to the cart.
- `GET /cart/total`: Get the total price of items in the cart.
- `POST /checkout`: Perform the checkout process.

Your goal is to simulate adding items to the cart, calculating the total price, and performing a checkout. You're free to use any programming language and library of your choice to complete this task.

Answer:

import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;
import static io.restassured.RestAssured.given;
import static org.testng.Assert.assertEquals;

public class CartCheckoutTest {

    private static final String API_URL = "http://localhost:8000";

    @BeforeClass
    public void setup() {
        RestAssured.baseURI = API_URL;
    }

    @Test
    public void testCartToCheckoutIntegration() {
        RequestSpecification request = given()
                .contentType("application/json")
                .body("{\"sessionid\": \"aa1dfk33\", \"product\": \"Product A\", \"quantity\": 2}");

        Response addResponse = request.post("/cart/add");
        assertEquals(addResponse.getStatusCode(), 201);

        Response totalResponse = given().get("/cart/total");
        assertEquals(totalResponse.getStatusCode(), 200);
        float totalPrice = Float.parseFloat(totalResponse.jsonPath().getString("total"));
        assertTrue(totalPrice > 1.0, "Total price is not greater than $1");

        request.body("{\"sessionid\": \"aa1dfk33\", \"total\": " + totalPrice + "}");
        Response checkoutResponse = request.post("/checkout");
        assertEquals(checkoutResponse.getStatusCode(), 200);
        assertEquals(checkoutResponse.getBody().asString(), "succeeded");
    }

}

## Chapter 5 - Regular Expressions

You are working on a project that involves parsing and validating phone numbers. Phone numbers in your application should have the following format: `(XXX) XXX-XXXX`, where each X represents a digit (0-9). Write a regular expression to validate phone numbers in the specified format.

Answer:

String regex = "^\\(\\d{3}\\) \\d{3}-\\d{4}$";

## Chapter 6 - CSS Selectors

Consider the following HTML snippet:

```html
 <div id="container">
    <h1>Welcome to Our Website</h1>
    <p class="highlight">This is a paragraph with class "highlight"</p>
    <p>This is another paragraph without any special class</p>
    <ul>
      <li>Item 1</li>
      <li class="selected">Item 2</li>
      <li>Item 3</li>
    </ul>
  </div>
```

The task:

a) Write a CSS selector to select the <h1> element with the text "Welcome to Our Website".

- Answer:
  h1:contains("Welcome to Our Website")

//Or in the case we do not use any libraries - h1

b) Write a CSS selector to select the <p> element with the class "highlight".

- Answer:
  p[class='highlight']

c) Write a CSS selector to select the second <li> element in the <ul> list.

- Answer:
  ul > li:nth-child(2)

d) Write a CSS selector to select the <li> element with the class "selected".

- Answer:
  ul > li[class='selected']

e) Write a CSS selector to select the <p> elements within the <div> with the id "container".

- Answer:
  div[id='container'] > p
