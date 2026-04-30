Query Processing + Cost Estimation, Week 8 Tutorial

Q1 ACB

Page oriented nested loop join
$\text{Cost}=\text{NPages}(\text{Outer})+\text{NPages}(\text{Outer})\cdot \text{NPages(Inner)}$


Block nested loops
What if we can store more than 2 pages in memory?

Use 2 pages for input/output buffers, and the other memory pages for holding as much of one relation as possible

$\text{Cost}(\text{BNJL})=\text{NPages}(\text{Outer})+\text{Nblocks}(\text{Outer})\cdot \text{NPages}(\text{Inner})$


Sort merge join
To sort
$\text{Cost}=\text{NPasses}\cdot\text{NPages}\cdot2$
To merge
$\text{Cost}=\text{NpagesR}+\text{NpagesS}$

Hash join

$\text{Cost}=\text{NPasses}

Question 3


Relation R contains 10000 tuples, 10 tuples a page, Relation S contains 2000 tuples and also has 10 tuples a page

Attribute b of relation S is the primary key for S
Both relations are stored as simple heap files
52 buffers available

3a) Cost of a page oriented simple nested loops

	$\text{Cost}=N+(N\times M)=200+(200\times 1000)=200200$

3 Buffer pages are required 1 for R, S and the output 

b) cost of a page using a block nested loop

We can get 50 pages of S in memory, that means we have 4 blocks

	$\text{Cost}=200+(4\times1000)=4200$
Requiring 52 buffer pages

c)
What is the cost of joining R and S using the sort-merge join algorithm
4000+800 Sorting both
+1200 Merging both
6000 Final
