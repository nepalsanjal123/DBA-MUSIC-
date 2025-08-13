/*
created by: Sanjay Nepal 
Date: 03/14/2025
Description: This query displays all customers first, last names, and email address.
*/

Select 
 FirstName AS [Customer First Name], 
 LastName AS [Customer Last Name],
 Email As [Email]
FROM 
Customer
ORDER BY 
FirstName ASC,
LastName DESC
LIMIT 10 ;

/*
Description: Customers who purchased two songs at $0.99 cents each. */

SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
total = 1.98
Order By 
InvoiceDate;


/*
Description: how many Invoice existed betwen 1.98 and 5 dollar. */

SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
total BETWEEN 1.98 AND 5.00
Order By 
InvoiceDate;

/*
Description: how many Invoice that we have that are exactly 1.98 or 3.96 */

SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
total IN (1.98,3.96)
Order By 
InvoiceDate;

/*
Description:how many Invoices were billed to brussels? */


SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
BillingCity = 'Brussels'
Order By 
InvoiceDate;

/* Description: how many invoices were billed to brussels, orlando and paris? */


SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
BillingCity IN ('Brussels','Orlando','Paris')
Order By 
InvoiceDate;

/* Description: how many invoices were billed in the cities that starts with B? */
-- % sign is just saying I don't care what comes next. --

SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
BillingCity Like 'B%'
Order By 
InvoiceDate;

/* Description: how many invoices were billed in the cities that have B anywhere in its name? */

SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
BillingCity Like '%B%'
Order By 
InvoiceDate;

/* how many invoices were billed on may 22, 2010? */
 SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
DATE(InvoiceDate)= '2010-05-22'
Order By 
InvoiceDate;

/* get all the invoices were billed after 2010-05-22 and have total of less than 3  */
 SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
BillingCity Like 'P%' OR BillingCity Like 'D%'
Order By 
InvoiceDate;

/* get all the invoices that are greater than 1.98 from any city that name starts with p or starts with D */
SELECT 
InvoiceDate,
BillingAddress,
BillingCity, 
total
FROM
Invoice
where 
total > 1.98 AND (BillingCity Like 'P%' OR BillingCity Like 'D%')
Order By 
InvoiceDate
;
/* WSDA Music Sales goal:
They want as many as customers to spend between $7.00 and $15.00 
sales Categories: 
Basleine Purchase: between $0.99 and $1.99
Low Purchase: Between $2.00 and $6.99
Target Purchase: between $7.00 and $15.00 
Top Performer: above $15.00 
*/

SELECT
InvoiceDate,
BillingAddress,
BillingCity,
total,
CASE
WHEN total<2.00 THEN 'Baseline Purchase'
WHEN total BETWEEN 2.00 AND 6.99 THEN 'Low Purchase'
WHEN total BETWEEN 7.00 AND 15.00 THEN 'Target Purchase'
ELSE 'Top Performer'
END AS PurchaseType
FROM
Invoice
where 
PurchaseType = 'Top Performer'
ORDER By
 BillingCity;

 /* Joins 
 */ 
 SELECT
 c.LastName,
 c.FirstName,
 i.InvoiceId,
 i.CustomerId,
 i.InvoiceDate,
 i.total
 FROM
 Invoice as i
 INNER JOIN
 Customer as c 
 on
i.CustomerId = c.CustomerId
ORDER BY 
c.CustomerId
;
/*
Create a Mailing List of Us
customers */
SELECT
FirstName,
LastName,
Address,
FirstName||' '||LastName||' '||Address||','||City||''|| PostalCode AS [Mailing Address],
length(postalcode),
substr(postalcode,1,5)AS [5 Digit Postal Code],
upper(firstname) AS [FirstName All caps],
lower(lastname) AS [LastName All lower]
FROM
Customer
WHERE
Country='USA'
;

/* calculate the ages of all employees 
*/ 
SELECT
LastName,
FirstName,
BirthDate,
strftime('%Y-%m-%d',Birthdate) As [Birthdate No Timecode],
strftime('%Y-%m-%d','now')- strftime('%Y-%m-%d',Birthdate) As Age
FROM
Employee
;

/* AggeWhar ar our all time gobal sales?
*/
Select 
sum(Total) AS [Total Sales], 
avg(total) as [Average Sales],
max(total) as [Maximum Sales],
min(total) as [Miminum Sales],
count(*) as [sales count]
From
Invoice;

/* Average invoice totals by city? */
SELECT
BillingCity,
round(avg(total),2)
From 
invoice 
Group By 
BillingCity
order by 
BillingCity;
 
/* Average invoice totals by city for only the cities that
starts with L? */
Select 
BillingCity,
round(avg(total),2)
From 
Invoice 
where 
BillingCity like 'L%'
group by 
BillingCity
order by 
BillingCity;

/* Average invoice total that are greater than $5.00? */
Select 
BillingCity,
round(avg(total),2)
From 
Invoice 
group by 
BillingCity
having 
 avg(total)>5
order by 
BillingCity;

/* Average invoice totals greater than $5.00 for cities that
starts with B? */

Select 
BillingCity,
round(avg(total),2)
From 
Invoice 
where 
BillingCity like 'B%'
group by 
BillingCity
order by 
BillingCity;

/* grouping by more than one field at a time | What are the
average invoice totals by billing countries and city? 
*/

SELECT
BillingCountry,
BillingCity,
round(avg(total),2)
from 
Invoice
group by 
BillingCountry,
BillingCity
order by 
billingcountry
;

/* calculate average spending 
amount of customer */

select 
billingcity as city,
round(avg(total),2) as Averagespending 
from 
invoice i 
group by 
billingcity 
order by 
city; 

/* Gather data about all invoices that are
 less than this average? */
 Select
 round(avg(total),2) AS [Average Total]
 from 
 Invoice;
 
 
 Select 
 InvoiceDate,
 BillingAddress,
 BillingCity,
 total 
 from 
 invoice
 where 
 total<
 (select avg(total) from invoice)
 order by 
 total DESC;
 
/* how is each individual city performing against the global average sales? */
select 
BillingCity,
avg(total),
(select avg(total) from invoice)[Global Average]

From 
invoice 
 group by 
 BillingCity
 order by 
 BillingCity 
;

/* Subqueries without Agregate functions */
select
InvoiceDate,
BillingAddress, 
BillingCity
From 
invoice
WHERE
InvoiceDate>
(SELECT
InvoiceDate
from 
Invoice
where 
invoiceid=251);

/* returning multiple values from a subquery */ 

SELECT
InvoiceDate,
BillingAddress,
BillingCity
FROM
Invoice
where
InvoiceDate in
(select 
InvoiceDate
from 
invoice
where 
invoiceid IN (251,252,254))
;

/* subqueries and DISTINCT| which tracks are not selling? */
SELECT
trackid, 
Composer,
name
from 
track 
where 
trackid 
NOT in

(SELECT
DISTINCT
TrackId
from 
InvoiceLine
order by 
TrackId);

/* This query idetifies tracks that have never been sold*/

SELECT 
t.trackid as "Track ID",
t.Name as "Track Name",
t.composer, 
g.name as Genre


FROM 
Track t
join Genre g on t.genreid = g.genreid
where 
t.TRACKID
not in 
(select 
distinct 
invoiceline.TRACKID
from 
invoiceline )
order by 
"Track Name"
;
/* Views */ 
DROP VIEW IF EXISTS V_AvgTotal;
CREATE VIEW V_AvgTotal AS
SELECT
avg(total) AS [Average Total]
From 
Invoice;

/* views and joins */
DROP VIEW if EXISTS V_Tracks_InvoiceLine;
create VIEW V_Tracks_InvoiceLine AS 
SELECT
il.InvoiceId,
il.UnitPrice,
il.Quantity,
t.Name,
t.Composer,
t.Milliseconds
FROM
InvoiceLine il 
INNER JOIN 
Track t
on
il.TrackId=t.TrackId;


 
 /* Inserting Data */ 
 
 INSERT INTO
  artist(Name)
VALUES ('Bob Marley');
  
/* Updating Data */ 

UPDATE 
Artist
SET Name = 'Damien Marley'
WHERE
ArtistId = 276;

/* DELETING Data */ 

DELETE FROM
Artist
where 
ArtistId = 276;

DROP View 
 V_AvgTotal;

 
SELECT Composer, COUNT(*) AS Count
FROM Track
GROUP BY Composer
ORDER BY Count DESC
LIMIT 10;

SELECT COUNT(*) AS CountLastNamesG
FROM Customer
WHERE LastName LIKE 'G%';
