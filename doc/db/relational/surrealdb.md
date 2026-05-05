# SurrealDB

https://surrealdb.com/


SurrealDB is the ultimate cloud database for tomorrow's applications
Develop easier.Build faster.Scale quicker.

With an SQL-style query language, real-time queries with highly-efficient related data retrieval, 
advanced security permissions for multi-tenant access, and support for performant analytical workloads, S
urrealDB is the next generation serverless database.


SurrealDB is an innovative NewSQL cloud database, suitable for serverless applications, jamstack applications, 
single-page applications, and traditional applications. It is unmatched in its versatility and financial value, 
with the ability for deployment on cloud, on-premise, embedded, and edge computing environments.
For a hassle-free setup, get started with SurrealDB Cloud in one-click.



https://surrealdb.com/features



Architecture
Single-node (in-memory)
COMPLETE
SurrealDB can be configured to run in a single-node memory-only runtime, enabling high-performance querying and data analysis. This is suitable for data which fits inside the memory of a single machine.

Single-node (on-disk)
COMPLETE
When data sizes are greater than the available memory on a single machine, on-disk storage can be used to enable larger data storage capabilities. Recently accessed data resides in memory with caching.

Distributed (multi-node)
FUTURE
For highly-available setups, SurrealDB can be run in a cluster using the RAFT consensus algorithm. The storage of the entire database must be less than the maximum possible storage of a single machine in the cluster.

Distributed (distributed kv-store)
COMPLETE
For highly-available and highly-scalable setups, SurrealDB can be run on top of a TiKV cluster, with the ability to horizontally scale to 100+ terabytes of data.

Platform
Multi-tenancy data separation
COMPLETE
Split data into namespaces and databases. There is no limit to the number of databases under each namespace, with the ability to switch between databases inside queries and transactions.

Schemafull or schemaless
COMPLETE
Store unstructured nested data with any columns, or limit data stored to only specific columns or fields. Get started quickly without having to define every column, and move to schemafull when your data model is defined.

Multi-table, multi-row transactions
COMPLETE
As a fully ACID compliant database, SurrealDB allows you to run transactions across multiple-rows, and across multiple different tables. There is no limit to the length of time a transaction can run.

Versioned temporal tables
FUTURE
Versioned temporal tables enable the option to 'go back in time' when querying your data. See how data looked before changes were made, or restore to a particular point-in-time.

Table fields
COMPLETE
When a table is defined as schemafull, only data allowed by defined fields will be stored. Table fields can be nested, and can be limited to a certain data type. VALUE clauses can be used to ensure a default value is always specified if no data is entered.

Table events
COMPLETE
Table events can be triggered after any change or modification to the data in a record. Each trigger is able to see the $before and $after value of the record, enabling advanced custom logic with each trigger.

Table indexes
COMPLETE
Table indexes improve data querying performance, and also allow for UNIQUE values in a table. Table indexes can be specified for a column, multiple columns, and have support for all nested fields including arrays and objects.

-- Specify a field on the user table
DEFINE FIELD email ON TABLE user TYPE string ASSERT is::email($value);

-- Add a unique index on the email field to prevent duplicate values
DEFINE INDEX email ON TABLE user COLUMNS email UNIQUE;

-- Create a new event whenever a user changes their email address
DEFINE EVENT email ON TABLE user WHEN $before.email != $after.email THEN (
CREATE event SET user = $this, time = time::now(), value = $after.email, action = 'email_changed'
);
Table constraints
COMPLETE
Each defined table field supports an ASSERT clause which acts as a constraint on the data. This clause enables advanced SurrealQL statements which can ensure that the $value entered is within certain parameters. Each clause is also able to see the $before and $after value of the record, enabling advanced custom logic with each trigger.

-- Specify a field on the user table
DEFINE FIELD countrycode ON user TYPE string
-- Ensure country code is ISO-3166
ASSERT $value != NONE AND $value = /[A-Z]{3}/
-- Set a default value if empty
VALUE $value OR 'GBR'
;
Full text indexing and filtering
FUTURE
The ability to define full-text indexes, with functionality to search through the full-text index on a table. Using a Apache Lucene query syntax, searches support field queries, wildcard searches, fuzzy searches, proximity searches, relevance matching, boolean operators, subquery grouping, and field grouping.

-- Define a fulltext index for 4 fields on the user table
DEFINE INDEX fulltext ON user
FULLTEXT
COLUMNS age, name, email, profile
;

-- Select all users who match a full text search query
SELECT * FROM user
WHERE search('profile:"SurrealDB" OR name:"Tobie"')
;
Pre-defined aggregate analytics views
COMPLETE
Aggregate views let you pre-compute analytics queries as data is written to SurrealDB. Similarly to an index, a table view lets you select, aggregate, group, and order data, with support for moving averages, time-based windowing, and attribute-based counting. Pre-defined aggregate views are efficient and performant, with only a single record modification being made for every write.

-- Drop all writes to the reading table. We don't need every reading.
DEFINE TABLE reading DROP;

-- Define a table as a view which aggregates data from the reading table
DEFINE TABLE temperatures_by_month AS
SELECT
count() AS total,
time::month(recorded_at) AS month,
math::mean(temperature) AS average_temp
FROM reading
GROUP BY city
;

-- Add a new temperature reading with some basic attributes
CREATE reading SET
temperature = 27.4,
recorded_at = time::now(),
city = 'London',
location = (-0.118092, 51.509865)
;
Data model
Basic types
COMPLETE
Support for booleans, strings, and numerics is built in by default. Numeric values default to decimal based numbers, but can be stored as int or float values for 64 bit integer or 64 bit floating point precision.

Empty values
COMPLETE
Values can be NONE, or NULL. A field which is NONE does not have any data stored, while NULL values are values which are entered but empty.

Arrays
COMPLETE
SurrealDB has native support for arrays, with no limit to the depth of nesting within arrays. Arrays can contain any other data value.

Objects
COMPLETE
Embedded object types are an integral feature of SurrealDB, with no limit to the depth of nesting for objects.

Durations
COMPLETE
Any duration from nanoseconds to weeks can be stored and used for calculations. Durations can be added to datetimes, and to other durations.

Datetimes
COMPLETE
Support for dates and datetimes in ISO-8601 format are supported. All dates are converted and stored in the UTC timezone.

Geometries
COMPLETE
SurrealDB makes working with GeoJSON easy, with support for Point, Line, Polygon, MultiPoint, MultiLine, MultiPolygon, and Collection values. SurrealQL automatically detects GeoJSON objects converting them into a single data type.

UPDATE city:london SET
centre = (-0.118092, 51.509865),
boundary = {
type: "Polygon",
coordinates: [[
[-0.38314819, 51.37692386], [0.1785278, 51.37692386],
[0.1785278, 51.61460570], [-0.38314819, 51.61460570],
[-0.38314819, 51.37692386]
]]
}
;
Futures
COMPLETE
Values which should be computed only when outputting data, can be stored as futures. These values are stored in SurrealDB as SurrealQL code, and are calculated only when output as part of a SELECT clause.

Casting
COMPLETE
In SurrealDB, all data values are strongly typed. Values can be cast and converted to other types using specific casting operators. These include bool, int, float, string, number, decimal, datetime, and duration casts.

UPDATE product SET
name = "SurrealDB",
launch_at = "2021-11-01",
countdown = <future> { launch_at - time::now() }
;
UPDATE person SET
waist = <int> "34.59",
height = <float> 201,
score = <decimal> 0.3 + 0.3 + 0.3 + 0.1
;
SurrealQL
SELECT, CREATE, UPDATE, DELETE statements
COMPLETE
Manipulation and querying of data in SurrealQL is done using the SELECT, CREATE, UPDATE, and DELETE methods. These enable selecting or modifying individual records, or whole tables. Each statement supports multiple different tables or record types at once.

-- Create a new article record with a specific id
CREATE article:surreal SET name = "SurrealDB: The next generation database";

-- Update the article record, and add a new field
UPDATE article:surreal SET time.created = time::now();

-- Select all matching articles
SELECT * FROM article, post WHERE name CONTAINS 'SurrealDB';

-- Delete the article
DELETE article:surreal;
RELATE statements
COMPLETE
The RELATE statement adds graph edges between records in SurrealDB. It follows the convention of vertex -> edge -> vertex or noun -> verb -> noun, enabling the addition of metadata to the edge record.

INSERT statements
COMPLETE
The INSERT statement resembles the traditional SQL statement, enabling users to get started quickly. It supports the creation of records using a VALUES clause, or by specifying the record data as an object.

-- Add a graph edge between user:tobie and article:surreal
RELATE user:tobie->write->article:surreal
SET time.written = time::now()
;

-- Add a graph edge between specific users and developers
LET $from = (SELECT users FROM company:surrealdb);
LET $devs = (SELECT * FROM user WHERE tags CONTAINS 'developer');
RELATE $from->like->$devs UNIQUE
SET time.connected = time::now()
;
INSERT INTO company {
name: 'SurrealDB',
founded: "2021-09-10",
founders: [person:tobie, person:jaime],
tags: ['big data', 'database']
};

INSERT IGNORE INTO company (name, founded)
VALUES ('SurrealDB', '2021-09-10')
ON DUPLICATE KEY UPDATE tags += 'developer tools'
;
Parameters
COMPLETE
Parameters can be used to store values or result sets, and can be used as stored parameters in client code.

Subqueries
COMPLETE
Recursive subqueries are useful for advanced querying or modification of values, whilst simplifying the overall query.

Nested field queries
COMPLETE
In SurrealQL any nested array or object value can be accessed and manipulated using traditional dot notation ., or array notation [].

Maths operators
COMPLETE
Maths operators can be used to perform complex mathematical calculations directly in SurrealQL.

Geospatial operators
COMPLETE
Geospatial operators enable geospatial containment and intersection operators on geospatial types.

Set operators
COMPLETE
Advanced set operators can be used to detect whether one or multiple values are included within an array. Fuzzy matching and regex matching can also be used for advanced filtering.

-- Use mathematical operators to calculate value
SELECT * FROM temperature WHERE (celsius * 1.8) + 32 > 86.0;

-- Use geospatial operator to detect polygon containment
SELECT * FROM restaurant WHERE location INSIDE {
type: "Polygon",
coordinates: [[
[-0.38314819, 51.37692386], [0.1785278, 51.37692386],
[0.1785278, 51.61460570], [-0.38314819, 51.61460570],
[-0.38314819, 51.37692386]
]]
};

-- Select all people whose tags contain "tag1" OR "tag2"
SELECT * FROM person WHERE tags CONTAINSANY ["tag1", "tag2"];

-- Select all people who have any email address ending in 'gmail.com'
SELECT * FROM person WHERE emails.*.value ?= /gmail.com$/;
Expressions
COMPLETE
SurrealQL supports fetching data using dot notation ., array notation [], and graph semantics ->. SurrealQL enables records to link to other records and traverses all embedded links or graph connections as desired. When traversing and fetching remote records SurrealQL enables advanced filtering using traditional WHERE clauses.

-- Select a nested array, and filter based on an attribute
SELECT emails[WHERE active = true] FROM person;

-- Select all 1st, 2nd, and 3rd level people who this specific person record knows, or likes, as separate outputs
SELECT ->knows->(? AS f1)->knows->(? AS f2)->(knows, likes AS e3 WHERE influencer = true)->(? AS f3) FROM person:tobie;

-- Select all person records (and their recipients), who have sent more than 5 emails
SELECT *, ->sent->email->to->person FROM person WHERE count(->sent->email) > 5;

-- Select other products purchased by people who purchased this laptop
SELECT <-purchased<-person->purchased->product FROM product:laptop;

-- Select products purchased by people in the last 3 weeks who have purchased the same products that we purchased
SELECT ->purchased->product<-purchased<-person->(purchased WHERE created_at > time::now() - 3w)->product FROM person:tobie;
Complex Record IDs
SurrealDB supports the ability to define record IDs using arrays and objects. These values sort correctly, and can be used to store values or recordings in a timeseries context.

// Set a new parameter
LET $now = time::now();
// Create a record with a complex ID using an array
CREATE temperature:['London', $now] SET
location = 'London',
date = $now,
temperature = 23.7
;
// Create a record with a complex ID using an object
CREATE temperature:{ location: 'London', date: $now } SET
location = 'London',
date = $now,
temperature = 23.7
;
Record ID ranges
SurrealDB supports the ability to query a range of records, using the record ID. The record ID ranges, retrieve records using the natural sorting order of the record IDs. These range queries can be used to query a range of records in a timeseries context.

// Select all person records with IDs between the given range
SELECT * FROM person:1..1000;
// Select all temperature records with IDs between the given range
SELECT * FROM temperature:['London', '2022-08-29T08:03:39']..['London', '2022-08-29T08:09:31'];
IDE language support
FUTURE
SurrealDB aims to include official language highlighting packages for Atom, Sublime Text, Visual Studio Code, and Vim.

Language Server Protocol
FUTURE
SurrealDB will support the Language Server Protocol which will help with code and query completion, and error highlighting for SurrealQL.

Learning fields
FUTURE
Learning fields bring machine-learning based decision-making directly into SurrealDB. When a value is not entered directly, then the field can be automatically calculated based on the machine learning analysis of other specified columns or fields.

-- Add a field which is based on other fields when a value is not specified
DEFINE FIELD won ON opportunity TYPE boolean
LEARN FROM
confidence,
person.tags,
person->sent->email.body
;

-- We are specifying the 'won' field, so it will use this to learn
CREATE opportunity SET won = true, confidence = 7, person = person:valeria;

-- We are specifying the 'won' field, so it will use this to learn
CREATE opportunity SET won = false, confidence = 3, person = person:jonathan;

-- Here the 'won' field is calculated with machine learning
CREATE opportunity SET confidence = 6, person = person:steven;
Functions
Array functions
COMPLETE
Functions for manipulation, joining, and diffing of arrays are built into SurrealDB as standard.

Http functions
COMPLETE
HTTP functions can be used for remote trigger events and webhook functionality.

Math functions
COMPLETE
Math functions can be used for complex statistical analysis of numbers and sets of numbers.

String functions
COMPLETE
Functions for string manipulation enable modification and processing of strings.

Parsing functions
COMPLETE
Parsing functions can be used for parsing and extracting individual parts or urls, emails, and domains.

Rand functions
COMPLETE
Random generation functions can be used to generate random values, numbers, strings, UUIDs, and datetimes.

Geo functions
COMPLETE
Geospatial functions can be used for converting between geohash values, and for calculating the distance, bearing, and area of GeoJSON data types.

Time functions
COMPLETE
Time functions can be used to manipulate dates and times - with support for rounding, and extracting specific parts of datetimes.

SELECT * FROM geo::hash::encode( (-0.118092, 51.509865) );
SELECT time::floor(created_at, 1w) FROM user;
Count functions
COMPLETE
SurrealDB supports general count functionality for counting total values, or for aggregate grouping. It's also possible to count only those expressions which result in a truthy value.

Validation functions
COMPLETE
Validation functions can be used to determine that field values match a certain pattern including hexadecimal, alphanumeric, ascii, numeric, or email addresses.

SELECT count(age > 18) FROM user;
SELECT email, is::email(email) AS valid FROM user;
Embedded JavaScript functions
COMPLETE
Javascript functions can be used for more complex functions and triggers. Each Javascript function iteration runs with its own context isolation - with the current record data passed in as the execution context or this value.

CREATE film SET
ratings = [
{ rating: 6, user: user:bt8e39uh1ouhfm8ko8s0 },
{ rating: 8, user: user:bsilfhu88j04rgs0ga70 },
],
featured = function() {
return this.ratings.filter(r => {
return r.rating >= 7;
}).map(r => {
return { ...r, rating: r.rating * 10 };
});
}
;
Permissions
Root access
COMPLETE
Root access enables full data access for all data in SurrealDB. Root access can be limited to specific IPv4 or IPv6 IP addresses.

Namespace access
COMPLETE
Namespace access enables full data access for all databases under a specific namespace. This access level is controlled using custom defined usernames and passwords.

Database access
COMPLETE
Database access enables full data access to a specific database under a specific namespace. This access level is controlled using custom defined usernames and passwords.

Scope access
COMPLETE
Scope access is the powerful functionality which enables SurrealDB to operate as a web database. Flexible authentication and access rules enable fine-grained access to tables and fields with the highest security, whilst ensuring that performance is affected as little as possible.

3rd party authentication
COMPLETE
If authentication with a 3rd party 0Auth provider is desired, specific tokens can be used for authentication with SurrealDB. ES256, ES384, ES512, HS256, HS384, HS512, PS256, PS384, PS512, RS256, RS384, and RS512 algorithms are supported.

-- Enable scope authentication directly in SurrealDB
DEFINE SCOPE account SESSION 24h
SIGNUP ( CREATE user SET email = $email, pass = crypto::argon2::generate($pass) )
SIGNIN ( SELECT * FROM user WHERE email = $email AND crypto::argon2::compare(pass, $pass) )
;
// Signin and retrieve a JSON Web Token
let jwt = fetch('https://api.surrealdb.com/signup', {
method: 'POST',
headers: {
'Accept': 'application/json',
'NS': 'google', // Specify the namespace
'DB': 'gmail', // Specify the database
},
body: JSON.stringify({
'NS': 'google',
'DB': 'gmail',
'SC': 'account',
email: 'tobie@surrealdb.com',
pass: 'a85b19*1@jnta0$b&!'
}),
});
Table permissions
COMPLETE
Fine-grained table permissions can be used to prevent users from accessing data which they shouldn't see. Independent permissions for selecting, creating, updating, and deleting data are supported, ensuring fine-grained control over all data in SurrealDB.

-- Specify access permissions for the 'post' table
DEFINE TABLE post SCHEMALESS
PERMISSIONS
FOR select
-- Published posts can be selected
WHERE published = true
-- A user can select all their own posts
OR user = $auth.id
FOR create, update
-- A user can create or update their own posts
WHERE user = $auth.id
FOR delete
-- A user can delete their own posts
WHERE user = $auth.id
-- Or an admin can delete any posts
OR $auth.admin = true
;
Connectivity
REST API
COMPLETE
All tables and data can be queried using a traditional Key-Value REST API. In addition, SurrealQL statements can be submitted to the REST API for custom query logic.

## Execute a SurrealQL query
localhost % curl -X POST https://api.surrealdb.com/sql
## Interact with a table
localhost % curl -X GET https://api.surrealdb.com/key/user
localhost % curl -X POST https://api.surrealdb.com/key/user
localhost % curl -X DELETE https://api.surrealdb.com/key/user
## Interact with a record
localhost % curl -X GET https://api.surrealdb.com/key/user/tobie
localhost % curl -X PUT https://api.surrealdb.com/key/user/tobie
localhost % curl -X POST https://api.surrealdb.com/key/user/tobie
localhost % curl -X PATCH https://api.surrealdb.com/key/user/tobie
localhost % curl -X DELETE https://api.surrealdb.com/key/user/tobie
SurrealQL over WebSocket
COMPLETE
SurrealQL querying and data modification is supported over WebSockets for bi-directional communication, and real-time updates.

JSON-RPC over WebSocket
COMPLETE
Querying and data modification is available using JSON-RPC over WebSockets, enabling easier implementation of 3rd party libraries.

GraphQL querying
FUTURE
Support for querying all data using GraphQL, with embedded and remote record fetching.

GraphQL mutations
FUTURE
Support for modifying and updating any data using GraphQL, depending on permissions.

GraphQL subscriptions
FUTURE
Support for subscribing to real-time data modification events, depending on permissions.

Tooling
Command-line tool
COMPLETE
The command-line tool can be used to export data as SurrealQL, import data as SurrealQL, and start a SurrealDB instance or cluster.

user@localhost % surreal

.d8888b.                                             888 8888888b.  888888b.
d88P  Y88b                                            888 888  'Y88b 888  '88b
Y88b.                                                 888 888    888 888  .88P
'Y888b.   888  888 888d888 888d888  .d88b.   8888b.  888 888    888 8888888K.
'Y88b. 888  888 888P'   888P'   d8P  Y8b     '88b 888 888    888 888  'Y88b
'888 888  888 888     888     88888888 .d888888 888 888    888 888    888
Y88b  d88P Y88b 888 888     888     Y8b.     888  888 888 888  .d88P 888   d88P
'Y8888P'   'Y88888 888     888      'Y8888  'Y888888 888 8888888P'  8888888P'


SurrealDB command-line interface and server

USAGE:
surreal [SUBCOMMAND]

OPTIONS:
-h, --help    Print help information

SUBCOMMANDS:
start      Start the database server
backup     Backup data to or from an existing database
import     Import a SQL script into an existing database
export     Export an existing database into a SQL script
version    Output the command-line tool version information
sql        Start an SQL REPL in your terminal with pipe support
help       Print this message or the help of the given subcommand(s)
SQL export
COMPLETE
Export all data as SurrealQL from a SurrealDB database for backup purposes. This includes authentication scopes, tables, fields, events, indexes, and data.

SQL import
COMPLETE
Import SurrealQL into a SurrealDB database in order to restore from a backup. This includes authentication scopes, tables, fields, events, indexes, and data.

Data backup
FUTURE
Export all data from SurrealDB as raw binary data. This will also support incremental binary backups for efficient backing up of SurrealDB clusters.

Docker container
COMPLETE
In addition to binary releases, SurrealDB is packaged as a Docker container for easy setup and configuration. All configuration is done with the command-line options. The Docker container can be used to start a SurrealDB instance or cluster, or to import and export data.

docker run --rm -p 8000:8000 surrealdb/surrealdb:latest start
User interface
PLANNED FOR 1.X
An easy-to-use interface with support for table-based views, SurrealQL querying, embedded object editing, and graph visualisation. The interface will be embedded within every SurrealDB executable.

SurrealDB Application Interface
Web app
PLANNED FOR 1.X
The interface will be available as a web app, installed on every instance of SurrealDB, and can be used for connecting to SurrealDB Cloud, or a local or enterprise cluster.

macOS app
PLANNED FOR 1.X
The interface will be available as an Tauri application for macOS. This can be used for connecting to SurrealDB Cloud, or a local or enterprise cluster.

Windows app
PLANNED FOR 1.X
The interface will be available as an Tauri application for Windows. This can be used for connecting to SurrealDB Cloud, or a local or enterprise cluster.

Libraries
Javascript
AVAILABLE
A real-time, fast SDK library for Javascript with bi-directional communication, and support for async / await programming styles.

Golang
AVAILABLE
A real-time, fast SDK library for Golang with bi-directional communication over WebSockets.

Deno
AVAILABLE
A real-time, fast SDK library for Deno with bi-directional communication, and support for async / await programming styles.

Rust
COMING SOON
A real-time, fast async-friendly SDK library for Rust with bi-directional communication over WebSockets.

WebAssembly
COMING SOON
A real-time, fast SDK library for Javascript with bi-directional communication, and support for async / await programming styles.

Node.js
COMING SOON
A real-time, fast native SDK library for Node.js with bi-directional communication, and support for async / await programming styles.

Python
COMING SOON
A real-time, fast SDK library for Python with bi-directional communication over WebSockets.

C
FUTURE
A SDK library for C, with support for data modification and querying.

PHP
FUTURE
A SDK library for PHP, with support for data modification and querying.

Swift
FUTURE
A real-time, fast SDK library for Swift with bi-directional communication.

Java
FUTURE
A real-time, fast SDK library for Java with bi-directional communication.

.NET
FUTURE
A real-time, fast SDK library for .NET with bi-directional communication.

Erlang
FUTURE
A real-time, fast SDK library for Erlang with bi-directional communication.

Ember.js
COMPLETE
A real-time, live-updating library for Ember.js, with authentication, model definition, embedded types, and remote fetching.

Angular
FUTURE
A real-time, live-updating library for Angular, with authentication, model definition, embedded types, and remote fetching.

React.js
COMPLETE
A real-time, live-updating library for React.js, with authentication, model definition, embedded types, and remote fetching.

Vue.js
FUTURE
A real-time, live-updating library for Vue.js, with authentication, model definition, embedded types, and remote fetching.

Apollo GraphQL
FUTURE
A real-time, live-updating library for Apollo GraphQL.

Dart
FUTURE
A real-time, fast SDK library for Dart with bi-directional communication over WebSockets.

Flutter
FUTURE
A real-time, fast SDK library for Flutter with bi-directional communication over WebSockets.

