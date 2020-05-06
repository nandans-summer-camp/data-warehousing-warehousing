# Analytics @FoosModels

You just got hired to the analytics team at Foo's Models, the premier online retailer of model cars and airplanes.

The thing is, you're the first person on the analytics team. So there's no infrastructure set up, you're in charge of everything.

Management wants the follow information from you and you've got to set up a process to get them the answers:

* Get the top 3 product lines that have proven most profitable (highest total profit)
* Get the top 3 products by most items sold
* Get the number of orders (amount of distinct orders) per day of the week
* List the customers who have bought more goods (in $ amount) than the average customer
* Get the average profit margin per product line (profit margin is profit divided by gross sales amount)


You can take a look at [connecting-remotely.md](connecting-remotely.md) for tips on setting up a better work environment for completing this task.


# 1. Mirror the operational database

The engineers don't trust you, so they won't let you access directly their production database. Instead, they've given you all the data from their operational database in a PostgreSQL dump file (`operational-dump.sql`).

You will need to create your own postgres database and load from the dump into your database, so that you can start doing something with it!

Use your AWS VM. Create a new database in your Postgres server (the one running inside the docker container). Load the data into it with:

``` shell
cat [DUMP FILE] | docker run -i --rm --net host postgres psql -h 0.0.0.0 -U postgres -d [NAME OF YOUR DATABASE]
```


# 2. Create a `.sql` file to create a set of tables in "star schema"

The operational database is in a totally annoying schema. You've got analytics queries to run and the data you want is split up all over the place and half of it isn't there.

You should create a new database with a new set of tables. You will create a `.sql` file to create these tables. This file should create the tables you would need to store the `orders` data we have been working with in "star schema".

You will need to be able to answer all the queries outlined in the `queries.sql` file in this assignment, so make your schema accordingly.


# 3. Create an `etl.py` file to perform an ETL process

Our ETL process will be to extract data from our observational database and put it into our star schema database.

NOTE: you can choose what part of the transformation you wish to do in SQL and what part you wish to do in Python. If you do most of the work with SQL queries, the python code simply makes queries from one database and inserts them into another.

However, you must be able to perform the ETL process entirely from a `.py` file, in python!


# 4. Create the queries outlined in the `queries.sql` file

These queries should run on your "star schema" data warehouse! They are answering the management's questions layed out in the beginning of this document.
