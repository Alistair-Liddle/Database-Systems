Storage and Indexing

Explain why/when we need search indexes
Explain the difference between the different type of indexing methods, and when they are appropriate




Files, Pages and Records
File Organisations
	Heaps
	Sorted



Terms relating to how the DBMS (Database management system) stores data:
	A file is the whole collection of data (e.g. an entity)
	A record is one row in the entity (e.g. Bob, 38, Sales)
	A page is how data is stored on the physical HDD/Storage Device: 
		A collection of records (number of records in a page depends on size)


We measure I/Os in 'pages', since pages take ac omparatively LONG time to retrive from storage vs accessing a record from INSIDE a page once it's loaded into memory

Example: If we have a file (table) with 100 records in it, and each page holds 10 records, how many I/Os does it take to access the whole file?

File Organisations: Heaped
	Records are not ordered in the File
	Pros/cons?
		Pages are linked to each other
		Not random access
		Fast to add
		Slow to search (cost = num pages)
Sorted
	Records are sorted on some attribute
	Pros/cons?
		Faster to search
		Slow to add/delete records

So what are indexes
	Indexes are completely separate from the data File itself
	We build a new data structure to help us extract records from the File using fewer I/Os, by strategically determining which Pages we need to access


B+ tree and Hash maps

B+ tree
	x,x+1 (directory root node num, num of immediate children)
	The tree points to a leaf node with only 1 piece of information and a POINTER to the information
	This means that as each node is a different page, in this simple tree it can be quite small leading to less page I/Os
	Will have a height of 3-4
		Better than binary search
		This is because it has more than 2 child nodes
	Great for accessing data
	Good for equal data

Hash Index
	Example uses a hash function of f(x) = x mod 4, hash functions can be anything
	Put the index data entries into a set of 'buckets', and then we know in which bucket to look later
	"SELECT name FROM emp WHERE postcode = 2007"
		2007 mod 4 = 3, so only need to look in bucket #3 of the index
	Good for equality statements
	Unusable for ranges, the DBMS won't even consider them

Composite keys
	Can be used for B+ and Hash


Clustering
	Clustered means the data File is sorted in the same way as the index (sorted on the search key). Makes range queries much more efficient




bitch but in red, ebk