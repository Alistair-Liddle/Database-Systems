Normalisation

Anomalies in a big table

How can we insert a subject with no students?
Or have a student who isn't studying anything yet

How can we remove all students from a course, without removing information about the course itself (e.g. Lecturer name)

How can we change the lecturer phone number for a class, without needing to update every row with that course in it

Insertion, Deletion, Update anomalies



Functional Dependencies + Determinants

A function dependency is when certain attributes of a table are 'dependent' on another
		'EmpName' and 'EmpDOB' are both dependent on 'EmpID'

A determinant is an attribute/set of attributes which uniquely determines the value of some other attributes
		'EmpID' is the determinant for 'EmpName' and 'EmpDOB'

Key attributes
	Minimal set of attributes that determine all of the other attributes
		EmpID and SaleNum together form the key for this relation

| EmpID | Empname | EmpDOB   | Salenum | SaleAmount |
| ----- | ------- | -------- | ------- | ---------- |
| 1     | Bob     | 1/1/1970 | 1       | 100        |
| 1     | Bob     | 1/1/1970 | 2       | 900        |
| 2     | Tiannan | 1/1/1999 | 1       | 566        |
| 2     | Tiannan | 1/1/1999 | 2       | 700        |

Partial functional dependency
	Is when an an attribute is only dependent on part of the key:
		Key was {EmpID,SaleNum}
		But EmpName/DOB depend only on EmpID


Transitive dependency
	A Transitive dependency is when a non-key attribute determines another non-key attribute
		Key is {EmpID} which determines DeptID. But DeptID also determines DeptName

| EmpID | EmpName | EmpDOB   | DeptID | DeptName    |
| ----- | ------- | -------- | ------ | ----------- |
| 1     | Bob     | 1/1/1970 | 1      | PCs         |
| 2     | Tiannan | 1/1/1999 | 2      | SmartPhones |

Armstrong's Axioms

These are the theoretical foundation for the rules we've just seen
	A={A1,A2,...,An} and B = {B1,B2,...,BN} and C = {C1,C2,...,Cn}
Reflexivity: $B \subseteq A \Rightarrow A \to B$
	Example: StudentID, name -> name
Augmentation: $A \to B \Rightarrow AC \to BC$
	StudentID, name, age -> name, age
Transitivity: $A \to B \text{ and } B \to C \Rightarrow A \to C$

| EmpID | Empname | EmpDOB   | Salenum | SaleAmount | DeptID | DeptName    |
| ----- | ------- | -------- | ------- | ---------- | ------ | ----------- |
| 1     | Bob     | 1/1/1970 | 1       | 100        | 1      | PCs         |
| 1     | Bob     | 1/1/1970 | 2       | 900        | 1      | PCs         |
| 2     | Tiannan | 1/1/1999 | 1       | 566        | 2      | SmartPhones |
| 2     | Tiannan | 1/1/1999 | 2       | 700        | 2      | SmartPhones |

key = {EmpID,SaleNum}
Empname is determined partially by EmpID
EmpDob is determined partially by EmpID
Saleamount is determined by EmpID and Salenum
DeptID is determined partially by EmpID
Deptname is transitivitly by DeptID

EmpID->Empname by reflexivity and transitivity

Normalisation

Normalisation involves breaking large tables down into smaller ones
	"Relation is normalised if all determinants are candidate keys"

Normal Forms
	First Normal Form
		1st = "Put it in a table"
	Second Normal Form
		2nd = "Remove partial dependencies"
	Third Normal Form
		3rd = "Remove transitive dependencies"

1st NF

| EmpID | Empname | EmpDOB   | Salenum | SaleAmount | DeptID | DeptName    |
| ----- | ------- | -------- | ------- | ---------- | ------ | ----------- |
| 1     | Bob     | 1/1/1970 | 1       | 100        | 1      | PCs         |
| 1     | Bob     | 1/1/1970 | 2       | 900        | 1      | PCs         |
| 2     | Tiannan | 1/1/1999 | 1       | 566        | 2      | SmartPhones |
| 2     | Tiannan | 1/1/1999 | 2       | 700        | 2      | SmartPhones |

2nd NF

| EmpID | Salenum | SaleAmount |
| ----- | ------- | ---------- |
| 1     | 1       | 100        |
| 1     | 2       | 900        |
| 2     | 1       | 566        |
| 2     | 2       | 700        |

| EmpID | Empname | EmpDOB   | DeptID | DeptName    |
| ----- | ------- | -------- | ------ | ----------- |
| 1     | Bob     | 1/1/1970 | 1      | PCs         |
| 2     | Tiannan | 1/1/1999 | 2      | SmartPhones |

3rd NF

| EmpID | Salenum | SaleAmount |
| ----- | ------- | ---------- |
| 1     | 1       | 100        |
| 1     | 2       | 900        |
| 2     | 1       | 566        |
| 2     | 2       | 700        |

| EmpID | Empname | EmpDOB   | DeptID |
| ----- | ------- | -------- | ------ |
| 1     | Bob     | 1/1/1970 | 1      |
| 2     | Tiannan | 1/1/1999 | 2      |

| DeptID | DeptName    |
| ------ | ----------- |
| 1      | PCs         |
| 2      | SmartPhones |