
PageRank is an algorithm developed by Google to rank websites in search results. It considers the quality and quantity of links pointing to a page. 
Websites with more high-quality backlinks are generally considered more authoritative and relevant, leading to a higher PageRank score and potentially a higher position in search results.
Project objectives:

    Implementation of PageRank using PySpark and Scala on GCP

    Showcase efficient pagerank computation on distributed computing platforms

Platforms and Languages used to manage data:

    GCP: Scalable cloud infrastructure for managing data processing pipelines.

    Dataproc: Managed service to run Apache Spark and Hadoop clusters for big data processing.

    PySpark and Scala: Choose between Python's PySpark for convenience or Scala for high-performance Spark development.

    GCP Storage: Efficient cloud storage for data (input.txt) and results.

The project runs PageRank on GCP:

    Cloud Storage: Holds data (input.txt) and code (Python's pagerank.py and Scala's ScalaPageRank.scala).

    Dataproc: Manages a Spark cluster for the computations.

    PySpark and Scala: Used to implement pagerank algorithm.

PageRank is an algorithm developed by Google to assess the importance of web pages. It considers the quality and quantity of links pointing to a page.

The Voting Analogy: Pages with more backlinks, especially from high-authority sites, receive a higher "vote" and thus a higher PageRank score.

Iterative Calculation: PageRank employs an iterative process. Initially, all pages have an equal rank. The algorithm then calculates how much "voting power" each page contributes to the pages it links to. These contributions are then used to update the ranks of all pages in the next iteration. This process continues for a set number of iterations, allowing the algorithm to converge on a final ranking.

Relevance and Authority: Websites with high PageRank scores are generally considered more relevant and authoritative in their respective domains. 

#PySpark Implentation
To conduct the implementation the environment has to be set up.

Set up Bucket

Store files in Bucket
<img width="361" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/9b51e41a-106a-4b31-88cf-b6ccbb6abda4">


Set up Cluster
<img width="748" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/0844a1ad-f9db-4875-8588-fdbd0acdfdca">

Run the Job

<img width="251" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/2bb50652-269d-41e6-a3bc-d42d103076a3">

Result
<img width="284" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/49337a09-ef5a-42fd-874e-d376752ffebd">

#Scala Implementation

To conduct the implementation the environment has to be set up.

Set up Bucket

Store input file in Bucket

Set up Cluster
<img width="395" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/d2e9a833-6ce0-4763-80a9-01f6a6147a6e">

Install Scala
```
$sudo apt-get update
$sudo apt-get install scala
$sudo apt-get install sbt
```
Setup PageRank Directory
```
$mkdir d
$mkdir pagerank
$cd pagerank/
$vi build.sbt<img width="440" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/6e5f20b3-581e-44cf-b9be-451f3e129e1c">
$vi src/main/scala/SparkPageRank.scala
```
<img width="359" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/d4048e55-4e68-410b-bf0a-c196bfa71d56">
<img width="353" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/d9d83b2d-536a-439e-ab00-abf0591005a5">
```
$sbt package
```
Run the Job
<img width="665" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/53e89748-d5ee-4c54-9f69-5da00fe45a43">

Result
<img width="284" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/65d55666-516f-4a67-8f35-c326cbe344e9">



