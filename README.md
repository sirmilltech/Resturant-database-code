-- This file is intended for notes
-- and for demonstrating how to work
-- with SQLite in Codespaces.

-- These two statements operate on the 
-- restaurant.db SQLite database.
SELECT * FROM Dishes;
SELECT * FROM Customers;

/*
-- Because our customer anniversary is coming up, we want to invite all our customer 
for a special celebration. all customer we have had in the previous years to our anniversary.
so that we can meet each others and enjoy our taste food.. 

We want to send an email to invite them, so get their names, address and email address

Generate a list of customer information like FirstName, lastname and email address, 
sort by lastname alphabetically.
*/
SELECT 
       FirstName AS 'First Name',
       lastname AS 'Last Name',
       Email AS Email
FROM Customers
ORDER by lastname ASC

/*
Create a table to record customer information and how many people will be present in the party, we need to use 
our database, we need to use customers ID from the customer table, we are not using the customer names or 
email since we had the information already
*/

--Create a table to store response info. CustomerID, PartySize,
CREATE TABLE AnniversityAttendees (
CustomerID INTEGER,
PartySize INTEGER
);

/*
CREATE THREE MENUS
1: ALL items sorted by prices, low to high
2: Appetizers and beverages by type
3: All items except beverages by type
*/
--1
SELECT 
       Name, Price
FROM Dishes
ORDER BY Price ASC

--2
SELECT Type, Name, Price, description
FROM Dishes
WHERE Type = 'Appetizer' OR Type = 'Beverage'
ORDER BY type

---3
SELECT
        Type AS 'Orders Type', Name AS 'NAME', 
        Price AS 'Orders Price'
FROM Dishes
WHERE Type IS NOT 'Beverage'
ORDER BY Type

/*
Customer request to join our loyality program, collect the information from customer and add the customer into 
information to a table in the company database

Insert the informations into the table, customers table. First Name, 
Lastname,Email,Address,city,state,Phone, Birthday.

*/
Insert INTO Customers (FirstName, LastName, Email, Address, City, State, Phone, Birthday, FavoriteDish) Values
('Mill', 'Tech', 'Milltech@datacalculations.com', '23, Talatpasa Mah', 'Esenyurt', 'Istanbul', '05387793556', '1984-08-01', '17')

SELECT FirstName AS 'NAME', Lastname,Email,Address,city,state,Phone, Birthday, FavoriteDish
FROM customers
WHERE CustomerID = '101'

/*
UPdating a customers address in the database. 
Example: Taylor Jenkins,
Old Address: 27170 6th Ave., Washington, DC
New Address: 74 Pine St., New York, NY
*/
UPDATE customers
SET  address = '74 Pine St.', 
     City='New York', 
     State= 'NY'
WHERE CustomerID = '26'

SELECT FirstName, CustomerID, Address
from customers
WHERE FirstName = 'Taylor' 
AND Lastname = 'Jenkins'

/*
Removing information like reservations from the database, 
*/

select *
  FROM Reservations
  join customers ON Reservations.CustomerID = Customers.CustomerID
  WHERE Customers.FirstName = 'Norby'
  AND Reservations.date > '2022-07-24'

delete from reservations where ReservationID = 2000

/*
atapley2j@kinetecoinc.com	
Asher Tapley
*/

select * 
from Customers 
join Reservations ON ReservationID = reservations.PartySize
where FirstName = 'Asher' AND lastname = 'Tapley'

INSERT INTO AnniversityAttendees (CustomerID, PartySize) Values ('92', '2')

UPDATE AnniversityAttendees
  SET PartySize = '4'
 WHERE PartySize = '2'

SELECT * FROM AnniversityAttendees
/*
IF a customer come to the resturant on june 14th about 6:15PM, with a group of three other people.
with a lastname Stevenson
Search for a reservation by name and look for similarity
join a customers table to reservations table to get the results
*/

SELECT 
        C.FirstName As 'First Name', c.lastname AS 'Last Name',
        R.PartySize AS 'Party Attendees', R.date AS 'Date of Event'
FROM Reservations R
JOIN Customers C ON R.CustomerID = C.CustomerID
WHERE C.lastname like 'St%' AND PartySize = '4'
ORDER By Date DESC
================================================================
SELECT
     Customers.FirstName, Customers.LastName, R.PartySize, R.date
FROM reservations R
join customers ON Customers.CustomerID = R.CustomerID
WHERE Customers.LastName like 'St%' 
AND PartySize = 4
ORDER BY R.Date DESC
/*
CREATE A RESERVATION.
With Sam McAdams - 12 August 2022, 6PM (5 people)
Email smac@kinetecoinc.com / (555)555-1212
*/

SELECT*
FROM Customers
WHERE Email = 'smac@kinetecoinc.com';

INSERT INTO Customers 
(FirstName, LastName, Email, Phone) Values
('Sam', 'McAdams','smac@kinetecoinc.com','555-555-1212');

INSERT INTO Reservations 
(CustomerID, Date, PartySize)
Values ('102', '2022-08-12 18:00:00', '5');

SELECT
       C.FirstName AS 'First Name', C.lastname AS 'Last Name', C.email AS 'E-mail',
       R.date AS 'DAte',R.PartySize AS 'Party Attendees', ReservationID, C.CustomerID
FROM Reservations R
JOIN  Customers C ON R.CustomerID = C.CustomerID
WHERE FirstName = 'Sam'
Order By PartySize 

/*
FOR: Loretta Hundey, 6939 Elka Place, House Salad,  Mini Cheeseburgers, Tropical Blue Smoothie.
         September 20, 2022, 2 pm
Challenge, create an order and items to it then fine the total......
s
     Find the customer, create the order record, Add items to the order, find the total cost.
TABLE TO INVOLVE: 
             Customers:  C.ID,Firstname, lastname,
             Dishes: Name, price,
             Orders: C.ID, Order date,
             ordersDishes: OrderID, DishID
*/
INSERT INTO Orders (CustomerID, OrderDate) 
      Values(70, '2022-09-20 14:00:00')

SELECT* 
FROM Orders
where CustomerID = '70' AND Orderdate = '2022-09-20 14:00:00'

INSERT INTO Dishes (Name, Description, Price, Type) 
       Values('House Salad', 'Mini Cheeseburgers, Tropical Blue Smoothie', '7.99', 'Main' )

select * 
from Dishes 
where DishID = '23'

INSERT INTO OrdersDishes (OrderID, DishID) 
        Values ('101', '23')
select* 
from OrdersDishes 
WHERE OrderID = '1001' and DishID = '23'

UPDATE
SET  


OrderID		1001	
CustomerID     70	
OrderDate  2022-09-20 14:00:00
DishID          23
OrdersDishesID  4022

select 
          C.Firstname AS 'First Name', C.lastname AS 'Last Name',
          SUM(Dishes.price) AS 'Total Cost' 
from Dishes
inner Join customers C on Dishes.DishID = Dishes.price
WHERE FirstName = 'Loretta'


INSERT INTO OrdersDishes (OrderID, DishID) 
       Values ('1001',(SELECT DishID FROM Dishes WHERE Name = 'House Saled')),
         ('1001',(SELECT DishID FROM Dishes WHERE Name = 'Mini Cheeseburgers')),
         ('1001',(SELECT DishID FROM Dishes WHERE Name = 'Tropical Blue Smoothie'))

SELECT 
     Sum(Dishes.Price)
FROM Dishes
JOIN OrdersDishes ON Dishes.DishID = OrdersDishes.DishID
WHERE OrdersDishes.OrderID ='1001'

/*  Challenge.
Set Cleo Goldwater's, favorite dish to Quinoa Salmon Salad
associate customers favoritedish and their records
*/
SELECT DishID
FROM Dishes
where name = 'Quinoa Salmon Salad'


UPDATE Customers
 Set FavoriteDish = (SELECT DishID FROM Dishes WHERE Name = 'Quinoa Salmon Salad' )
   WHERE CustomerID = 42

/* CHALLENGE
Generate a list of the customers who have placed the most to-go orders, and count how many orders they have made.
Firstname, lastname and email.
sort by the number of orders.

Hint: Customers Table and Orders table
*/
SELECT count(Orders.orderID) AS 'OrderCount', C.FirstName AS 'First Name', 
      C.LastName AS 'Last Name', C.Email AS 'E-mail'
FROM Orders
JOIN Customers C ON Orders.CustomerID = C.CustomerID
Group by Orders.CustomerID
Order By OrderCount DESC
limit 6
