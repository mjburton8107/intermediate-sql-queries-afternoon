# intermediate-sql-queries-afternoon

For today we will be practicing inserting and querying data using SQL.

Here is a website that will let us write queries to interact with some data. http://jxs.me/chinook-web/

On the left are the Tables with their fields. The right is where we will be writing our queries. The bottom is where we will see our results.

Any new tables or records that we add into the database will be removed after you refresh the page. So if you're wondering where you data went, that may be why.

Use www.sqlteaching.com or sqlbolt.com as resources for the missing keywords you'll need.

## Practice joins

### Use at least one join for all of the following

__Examples__
```
Select [Column names] from [Table] [abbv] join [Table2] [abbv2] on abbv.prop=abbv2.prop where [Conditions]

Select a.Name, b.Name from someTable a join anotherTable b on a.someid=b.someid
Select a.Name, b.Name from someTable a join anotherTable b on a.someid=b.someid where b.email='e@mail.com'
```

* Get all invoices where the unit price on the invoice line is greater than $0.99

SELECT * FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
WHERE UnitPrice > 0.99

* Get all invoices and show me their invoice date, customer first and last names, and total
SELECT Invoice.InvoiceDate, Customer.FirstName, Customer.LastName, Invoice.Total
FROM Invoice
JOIN Customer ON Invoice.CustomerId = Customer.CustomerId


* Get all customers and show me their first name, last name, and support rep first name and last name (support reps are on the Employees table)
SELECT Customer.FirstName as CustomerFirst, Customer.LastName as CustomerLast,
Employee.FirstName as EmployeeFirst, Employee.LastName as EmployeeLast
FROM Customer
JOIN Employee ON Customer.SupportRepId = Employee.EmployeeId


* Get all Albums and show me the album title and the artist name

SELECT Album.Title, Artist.Name FROM Album
JOIN Artist ON Artist.ArtistId = Album.ArtistId


* Get all Playlist Tracks where the playlist name is Music

SELECT PlaylistTrack.TrackId as TrackID, Playlist.name as PlaylistName FROM PlaylistTrack
JOIN Playlist ON Playlist.PlaylistId = PlaylistTrack.PlaylistId
WHERE PlaylistName = 'Music'

* Get all Tracknames for playlistId 5

SELECT Track.Name, PlaylistTrack.PlaylistId FROM Track
JOIN PlaylistTrack ON PlaylistTrack.TrackId = Track.TrackId
WHERE PlaylistTrack.PlaylistId = 5



* Now we want all tracknames and the playlist name that they're on (You'll have to use 2 joins)
SELECT Track.Name as TrackName, Playlist.Name as PlaylistName FROM Track
JOIN PlaylistTrack ON PlaylistTrack.TrackId = Track.TrackId
JOIN Playlist ON Playlist.PlaylistId = PlaylistTrack.PlaylistId


* Get all Tracks that are alternative and show me the track name and the album name (2 joins)

SELECT Track.Name as TrackName, Album.Title as AlbumTitle
FROM Track
JOIN Album ON Album.AlbumId = Track.AlbumId
JOIN Genre ON Track.GenreId = Genre.GenreId


#### Black Diamond :

* Get all tracks on the playlist(s) called Music and show their name, genre name, album name, and artist name (at least 5 joins)


## Practice nested queries

### Use no joins for the following queries.  Use only nested queries.

__Examples__
```
Select [Column names] from [Table] where columnId in (select columnId from [Table2] where [Condition])

Select Name, email from athlete where athleteId in (select personId from pieEaters where flavor='Apple')
```

* Get all invoices where the unit price on the invoice line is greater than $0.99

SELECT *
FROM Invoice
WHERE InvoiceId IN
(SELECT InvoiceId FROM InvoiceLine WHERE UnitPrice > 0.99)

* Get all Playlist Tracks where the playlist name is Music

SELECT *
FROM PlaylistTrack
WHERE PlaylistId IN
(SELECT PlaylistId FROM Playlist WHERE Name = 'Music')

* Get all Tracknames for playlistId 5

SELECT Name
FROM Track
WHERE TrackId IN
(SELECT TrackId FROM PlaylistTrack WHERE PlaylistId = 5)

* Get all tracks where the genre is comedy

SELECT *
FROM Track
WHERE GenreId IN
(SELECT GenreId FROM Genre WHERE Name = 'Comedy')


* Get all tracks where the album is Fireball

SELECT *
FROM Track
WHERE AlbumId IN
(SELECT AlbumId FROM Album WHERE Title = 'Fireball')

* Get all tracks for the artist queen Queen (2 nested subqueries)

SELECT Name FROM Track
WHERE AlbumId IN
(SELECT AlbumId FROM Album WHERE ArtistId IN
 (SELECT ArtistId from Artist WHERE Name = "Queen"))



## Practice updating Rows

__Examples__
```
Update [Table] set [column1]=[value1], [column2]=[value2] where [Condition]

Update Athletes set sport='Picklball' where sport='pockleball'
```

* Find all customers with fax numbers and set those numbers to null

UPDATE Customer SET Fax=Null WHERE Fax IS NOT Null

* Find all customers with no company (null) and set their company to self

UPDATE Customer SET Company='Self' WHERE Company IS Null

* Find the customer `Julia Barnett` and change her last name to `Thompson`

--note, I ran a query to check that she was the only Barnett in the DB

UPDATE Customer SET LastName='Thompson' WHERE LastName='Barnett'

* Find the customer with this email `luisrojas@yahoo.cl` and change his support rep to rep 4

UPDATE Customer SET SupportRepId=4 WHERE Email='luisrojas@yahoo.cl'

* Find all tracks that are of the genre `Metal` and that have no composer and set the composer to be 'The darkness around us'

UPDATE Track SET Composer='The darkness around us' WHERE Composer is Null



Once you're done with all of those refresh your page to blow away your changes to the database!

## Group by

* Find a count of how many tracks there are per genre

SELECT count(*), GenreId FROM Track
GROUP BY GenreId*

* Find a count of all Tracks where the Genre is pop
SELECT count(*) FROM Track
WHERE GenreId = 9*

* Find a list of all artist and how many albums they have

SELECT Artist.Name, count(Album.Title)
FROM Artist
JOIN Album ON Album.ArtistId = Artist.ArtistId
GROUP BY Artist.Name

## Use Distinct

* From the tracks table find a unique list of all composers
SELECT DISTINCT Composer FROM Track

* From the Invoice table find a unique list of all Billing postal codes

SELECT DISTINCT BillingPostalCode FROM Invoice

* From the Customer table find a unique list of all companies

SELECT DISTINCT Company FROM Customer



## Delete Rows

Always do a select before a delete to make sure you get back exactly what you want and only what you want to delete!

* Remove all pop tracks from the tracks table
DELETE FROM Track
WHERE GenreId = 9
FOREIGN KEY constraint failed

* Remove all tracks by `Santana`

DELETE FROM Track
WHERE Composer = 'Santana'

* Remove all of the rest of the tracks, yes all of them.
DELETE FROM Track

Now refresh your browser to reset all your data



## eCommerce simulation

Let's simulate an e-commerce site.  We're going to need users, products, and orders.

Users need a name and an email.
Products need a name and a price
Orders need a ref to product.
All 3 need primary keys.

Add some data to fill up each table (write down your schema since you won't see it on the side).  You'll need to insert products before you can link them.

Add 2 users, multiple products and multiple orders.

Run some queries against your data:

* Get all products for the first order
* Get all orders
* Get the total cost of an order (sum the price of all products on an order)

## Add foreign key to existing table

Orders have products, but someone needs to place the order.

Add a ref from Orders to Users.  
Add some users.
Update the Orders table to link the a user to each order.

Run some queries against your data:

* Get all orders for a user
* Get how many orders each user has
* __Black Diamond:__ Get the total spend on all orders for each user
