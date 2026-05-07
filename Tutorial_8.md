Query Optimisation

Week 9

Reduction factors

SELECT attribute list
FROM relation list
WHERE term_1 AND ... AND term_2
 RF associated with each term reflects the impact of the term in reducing result size
Result Cardinality = Max (Ntuples) $\cdotp\Uppi$(The reduction factors)

Assume independence of attributes of tuples, uniform distrivution within each attribute

COL = Value
	RF = 1/Nkeys(Col)
Col > Value
	RF=(High(col)-value)/(High(col)-Low(col))
Col < value
	RF = (value-Low(Col))/(High(Col)-Low(Col))
Col_A=Col_B (for joins)
	RF=1/(Max(NKeys(Col_A),NKeys(Col_B)))
In no information about Nkeys or intervals, use a magic number 1/10
	RF=1/10


Single relation plan costs
1. Sequential (heap) scan of data file:
	Cost=Npages(R)
2. Index selection over a primary key (just a single tuple)
	Cost(B+Tree)=Height(I) +1, height is the index height
	Cost(HashIndex)=Probecost(I)+1, ProbeCost(I)~1.2
3. Clustered index matching one or more predicates:
	Cost(B+Tree)=Npages(I) \* **Npages**(R))\*Pi(RF)
	Cost(HashIndex)=**Npages**(R)\*Pi(RF)\*2.2
4. Non clustered index matching one or more predicates:
	Cost(B+Tree)=(NPages(I)+**NTuples**(R))\*Pi(RF)
	Cost(HashIndex)=**NTuples**(R)\*Pi(RF)\*2.2

Q1)
	a) sal>20000
		RF = 3/5
			$\frac{\text{High}(I)-\text{value}}{High(I)-Low(I)}$
		The unclustered B+ tree index on sal, with costs
			RFs\*Ntuples(R)+NPages(I))
			=$0.6\cdot((20\cdot10000)+500)$
			=$120300$ I/Os
		Full table scan with cost 10000 I/Os
	b) age=25
		RF=1/40
		Clustered B+ tree on (age,sal)
			RFs\*Npages(R)+NPages(I)
			=1/40 \*(500+10000)
			=263 I/Os
		Unclustered hash index on age
			=RFS\*hash lookup cost \* NTuples
			=1/40 \* 2.2 \* (20\* 10000)
			=11000 I/Os
		Full table scan with cost 10000 I/Os
	c) age>30
		RF = 3/4
		A clustered B+ tree index on Age sal
			0.75 \* (500+10000)
			=7875
		Full table scan with cost 10000
	d) eid=1000
		RF=1/200000
		Cost = hash lookup + 1 data page index = 2.2
	e) sal>20000 and age > 30
		RF = 3/5 \* 3/4
		= 9/20
		Clustered B+ tree index on (age,sal)
			RFs \* (10000+500)
			=4725 I/Os
		Unclustered B+ tree index on sal
			RFs of matching conditions \* (Ntuples(R)+NPages(I))
			=0.6\*(20\*10000+500)
			=120,300 I/Os
		Full table scanm with cost 10000 I/Os


Multi relation plan estimates
	Left deep joins
		"On the fly" reading
	Choose join algorithm at each node
	Calculate the cost
	Step
		1. Calculate the cost of Lowest join using the access pattern given
		2. Calculate number of pages resulting from the join you just did
		3. Use this number of pages to calculate cost of next join up the tree, and remember -1 access cost due to pipelining
		4. Go back to step 2 if there are further joins up the tree, else sum up costs of all joins

Question 2)
	a) SELECT 
		Cost(DeptBTEmp)=Npages(Dept) + Npages(Dept) \* NPages(Emp)
		=125125
		RF = 1/500
			Max of 1/NKeys(DeptID)
		Number of resulting tuples for Dept JOIN Emp
			1/500 \* 5000 \* 20000 = 200000
		Number of Pages for Dept JOIN Emp = 200000/100
		Cost to join with Proj = Npages(Dept BT Emp) \* NpAges Proj = 2000 \* 100 = 200000 I/I
			Don't need to read outer again because of pipelining
		Total Cost = 125 + 125000 + 200000 = 325125 I/O
	b) NLJ and HJ
		Cost(DeptBTEmp)=Npages(Dept) + Npages(Dept) \* NPages(Emp)
		=125125
		RF = 1/500
			Max of 1/NKeys(DeptID)
		Number of resulting tuples for Dept JOIN Emp
			1/500 \* 5000 \* 20000 = 200000
		Number of Pages for Dept JOIN Emp = 200000/100
		Cost to join with Proj = 2 \* NPAges(Dept BT Emp) + 3 \* Npages(Proj) - Npages(Dept JOIN emp)
			subtract Dept because of pipelining
		=4300
		Therefore total cost = 125125+4300
	c) SMJ and HJ
		
