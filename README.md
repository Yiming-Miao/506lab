# Molecule-Database
## Description
The goal of our group is to build a molecule database that can be used to add molecules and find molecules with at least a command-line user interface. Moreover, the database should be able to efficiently handle at least 10,000 molecules and be able to efficiently search for a given molecule up to graph isomorphism.

## Group Members
Yufeng Lin <br/>
Yiming Miao <br/>
Tingyi Zhang <br/>
Xiaoyu An <br/>
Qing Han <br/>

## Data Structure
We used SQLite library to build our database. In our database, there are four tables.<br> 
The first table is the periodic table.

｜EID｜INT(PRIMARY KEY)｜Atomic number｜
｜ ------ ｜ ------ ｜ ------ ｜
｜ENAME｜VARCHAR｜Element name｜
｜SYMBOL｜VARCHAR｜Element symbol｜

So generally a molecule is stored in the form of adjacency list. But in computing graph isomorphism, we use the Graph class of org.jgrapht library. The reason why we choose that is rather than implement a graph class ourself, we can take advantage of a user-friendly and powerful public library, which won’t bother us on thinking of the big picture of our algorithm.

- - -
## Methods Implementation:
### Add Molecule:
Converts a text file into a graph represented by adjacency list and stores it into the database.

### Find Molecule:
Implemented the algorithm in [4]. Used org.jgrapht library to build the molecule graph from text file and from database. In [4], it mentioned an optimized backtracking for isomorphism that achieved linear performance. This algorithm is about finding the mapping for each node in the subgraph in the main graph using recursive comparison of neighboring nodes. The recursion goes on until it find correct mapping for all the nodes in the subgraph. This algorithm is very useful for both graph isomorphism and subgraph search, which is perfect for our task. But we only realized graph isomorphism for duplicate node labels, which means in our algorithm, we ignored the name of vertices. But we also implemented a getFormula() function to make sure of finding accuracy. 

### Additional feature implementation

### Stand-alone Graphical User Interface
The GUI provides access to 3 functions: add and find molecules through txt file path, and display database statistics, using Swing framework. The corresponding function in database will be invoked based on the action in GUI.  To add molecules to the database, click the “Browse” to choose the txt file from user computer and the name of the file will be shown in the text filed for users to check. If the molecule already exists, the notification dialog will notice the user the compound is already in the database, otherwise, it will inform user adding successfully. For finding molecules is similar to adding, the result for finding will be displayed in notification dialog. And the statistic of database can be seen by clicking the button “get database statistics”. It is accomplished by keeping track of the size of the database.

### Download 1,000 known compounds from Pubchem database
We download data from PubChem using PubChem API and creating an HttpURLConnection object, and pull data in JSON file. The compounds are added by increasing the ID of them up to 1000 compounds.

### Search for the most similar molecule to a given molecule
 There are two main cases when determining molecule similarity. The input molecule will be compared to the database existing molecule one by one.
The first case is if the input molecule has the same chemical formula as a current molecule in the database. The formula can be got by a getFormula function as a hashmap form, then we use the back-tracking algorithm (graph isomorphic) to see if they also contain the same structure. If so, we return the identical structure as found in the SQL database. If not, we return the first structure collected by the second case. This case will happen when there is a chemical formula in the database, but the exact molecule isn't detected, as this will represent the most similar chemical structure of the input molecule.
The second case is if the input molecule has a different chemical formula with the current molecules in the database, we see if there exists a compound with the same atoms as the input molecule, and we set a flag to mark whether we find such a molecule in the database. After loop all the existing molecules, if the flag is set, we return the last molecule that flag is true, as this would be the most similar to the input molecule. If not, we do not declare any molecules as similar, as there would be a large percentage of pre-existing molecules as candidates for the most similar to the input-molecule.

### Implement subgraph search 
In subgraph search,  input is represented as a graph we would like to find. The idea implemented in subgraph search is comparing the induced subgraph with the input. We create a corresponding induced subgraph with AsSubgraph class with molecule's vertex and edge set. Then iterate through the database and compare the induced subgraph with our input. For filtering: since subgraph also has its own vertex set, it’s easy to filter those unqualified graphs which do not have one of the vertices (atoms in this project) in the subgraph vertex set. <br/>
Another method of subgraph search is to check if they have the same DFS iteration, if they have  the same DFS result, it’s easy to imply that input and molecule have the same subgraph.

## Changes made after midterm report

1. Used GUI rather than web interface for user interface <br/>
2. Since the algorithm we used for graph isomorphism is subgraph-search-friendly, we also implemented subgraph search.

## Code
All codes and data needed are included in the group's BitbucketRepos.
- - -
## Work Breakdown
### Qing Han:
<p>
Built and tested the Graphical User Interface and connected it with the database functions.<br/> 
Modified the SQLiteJDBC to link the functions with GUI.<br/>
Wrote the function to get the statistics of the database.
</p>

### Yiming Miao:
<p>
Downloaded compounds from existing online databases.<br/>
Finished the design and implementation of the database.<br/>
Implemented inserting compound from text file to the database.<br/>
Presented experiments and speeded up the searching process.
</p>

### Xiaoyu An:
<p>
Implemented findMostSimilar function: look for 4 cases to find the most similar molecule given an input molecule file<br/>
Implemented findSubgraph function: find one molecule that is the subgraph of the input molecule file<br/>
implemented readCSV function, which will read the periodic table into the database.
</p>

### Tingyi Zhang
<p>
Perfected addMolecule() function and the database.<br/>
Implemented findMolecule() function in class GraphIso using optimized backtracking isomorphism mentioned in the reference.<br/>
Implemented all command-line operations.<br/>
Synthesized and tested all units and the system.
</p>

### Yufeng Lin
<p>
Independent module testing <br/>
Implemented subGraphSearch method which based on comparing the induced subgraph of existed molecules.<br/>
Implemented web-interface (Aborted for final deliverable)
</p>

- - -
## Reference
<p>
[1] Price, C. C. (1971). Geometry of molecules. New York: McGraw-Hill. <br/>
[2] Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). Introduction to algorithms. Cambridge (Mass.): MIT Press. <br/>
[3] J. R. Ullmann. 1976. An Algorithm for Subgraph Isomorphism. J. ACM 23, 1 (January 1976), 31–42.<br/>
[4] Fu, L., & Chandra, S. (2012). Optimized backtracking for subgraph isomorphism. International Journal of Database Management Systems, 4(6), 1. <br/>
[5] F. Hao, J. Daugman and P. Zielinski, "A Fast Search Algorithm for a Large Fuzzy Database," in IEEE Transactions on Information Forensics and Security, vol. 3, no. 2, pp. 203-212, June 2008.
</p>
 



