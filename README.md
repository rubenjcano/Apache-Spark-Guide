<h1 align="center">
  <img width="230" height="120" alt="Apache Spark Guide" src="https://github.com/user-attachments/assets/c50f15bb-818c-4f87-a577-bee7539aeffa" /><br/>
  Apache Spark Guide for Data Engineers 🚀
</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Spark-Guide-orange?style=flat-square" alt="Spark Guide"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License"/>
  <img src="https://img.shields.io/badge/PySpark-Supported-blue?style=flat-square" alt="PySpark Supported"/>
</p>

<p align="center">
  <img width="1266" height="460" alt="Apache Spark" src="https://github.com/user-attachments/assets/e883f51f-8f48-4124-b976-a2edc6067a94" />
</p>

## 📚 Overview

This repository provides a comprehensive guide to **Apache Spark** for aspiring Data Engineers. It covers core concepts, hands-on examples, and best practices for working with Spark in distributed environments.

---

## 📦 Quickstart

```bash
# Install PySpark
pip install pyspark

# Run a simple job
python examples/simple_job.py
```

---

## 📑 Table of Contents

<details>
<summary><strong>Expand to view sections</strong></summary>

1. [Introduction](#introduction)
2. [Installation & Setup](#installation--setup)
3. [Spark Core Concepts](#spark-core-concepts)
4. [PySpark](#pyspark)
5. [Spark SQL](#spark-sql)
6. [Spark Streaming](#spark-streaming)
7. [Machine Learning with MLlib](#machine-learning-with-mllib)
8. [Integration with Cloud Platforms](#integration-with-cloud-platforms-azure-gcp-aws)
9. [Sample Projects](#sample-projects)
10. [Resources](#resources)
</details>

---

## ✨ Introduction

### What is Apache Spark?

**Apache Spark** is an **open-source**, distributed computing framework designed for **fast** and **scalable data processing**. It enables organizations to **analyze massive datasets efficiently** by leveraging **in-memory computation** and **parallel processing** across **clusters**. Unlike traditional systems such as **Hadoop MapReduce**, which rely heavily on **disk I/O**, Spark stores **intermediate data in memory**, making it **up to 100 times faster** for certain workloads.


Spark provides a **unified platform** for diverse data processing tasks:

- 🗃️ **Batch processing**
- ⚡ **Real-time streaming**
- 💻 **Interactive queries**
- 🤖 **Machine learning**

Its architecture supports horizontal scaling and in-memory computations. With built-in libraries:

- **Spark SQL** (SQL queries)
- **MLlib** (machine learning)
- **GraphX** (graph processing)
- **Structured Streaming** (real time analytics)

Spark simplifies complex analytics workflows.

---

### How Apache Spark Works? ⚙️

Apache Spark is a distributed computing framework designed for large-scale data processing. Here’s a more detailed look at how a typical Spark application runs:

1. **Application Submission:**  
   The user submits code to Spark through either a `SparkContext` (the original entry point for RDDs) or a `SparkSession` (the unified entry for DataFrames, SQL, and more).  
   - **Example (PySpark):**
     ```python
     # Using SparkContext
     from pyspark import SparkContext
     sc = SparkContext(appName="MyApp")

     # Using SparkSession (recommended)
     from pyspark.sql import SparkSession
     spark = SparkSession.builder.appName("MyApp").getOrCreate()
     ```

2. **Driver Program and Cluster Manager:**  
   The driver program orchestrates execution, communicating with a **Cluster Manager** (such as **YARN** or **Kubernetes**). The cluster manager allocates resources and launches executors on worker nodes.  
   - The driver keeps track of the application’s logic and divides work into tasks.

3. **Executors and Task Execution:**  
   **Executors** are launched on worker nodes. They perform the actual computation and hold data in memory if required (for caching or iterative algorithms).  
   - Executors run tasks in parallel, maximizing resource utilization.
   - They also provide in-memory computation for better performance.

4. **RDD & Transformation Pipeline:**  
   Spark’s core abstraction is the **RDD (Resilient Distributed Dataset)**—an immutable and distributed data collection. Users define transformations (like `filter` or `map`) that are _lazy_ and only executed when an action is triggered.
   - **Example:**
     ```python
     rdd = spark.sparkContext.parallelize([1, 2, 3, 4, 5])
     # Define transformations (lazy evaluation)
     transformed_rdd = rdd.filter(lambda x: x > 2).map(lambda x: x * 2)
     # Trigger action to execute transformations
     result = transformed_rdd.collect()
     ```
   - Spark constructs a **Directed Acyclic Graph (DAG)** representing the logical execution plan, dividing the work into stages based on required data shuffles.

5. **Stages, Tasks, and Parallelism:**  
   The DAG is divided into _stages_ depending on whether shuffles (redistribution of data) are needed. Each stage consists of multiple _tasks_ distributed across the executors, running in parallel.
   - Intermediate results may be cached to speed up iterative jobs.

6. **Fault Tolerance via Lineage:**  
   Spark ensures fault tolerance by tracking RDD _lineage_—the sequence of transformations used to build an RDD.
   - If a data partition is lost (e.g., due to node failure), Spark uses this lineage to recompute only the lost data.
   - Spark optimizers like **Catalyst** (for SQL/DataFrames) and **Tungsten** (for memory and execution efficiency) further enhance performance.

7. **Built-in Libraries and Extensibility:**  
   Spark provides built-in support for:
   - **Spark SQL:** Structured data queries
   - **MLlib:** Machine learning algorithms
   - **GraphX:** Graph processing
   - **Structured Streaming:** Real-time analytics

> **In summary:**  
> - Spark coordinates distributed data processing via a driver.  
> - The cluster manager handles resource management and executor launch.  
> - Executors run tasks and can cache intermediate results for efficiency.  
> - RDD lineage enables fault tolerance.  
> - High-level libraries make Spark versatile for batch, streaming, machine learning, and graph workloads.


### 1. Spark Architecture

![Spark Architecture](https://github.com/user-attachments/assets/6b3e6edd-b301-44b2-b216-87c23ebe2434)

This diagram illustrates the core components of Apache Spark’s architecture and their interactions. At the center is the Driver Program, which runs the SparkContext and coordinates the entire application. The driver communicates with the Cluster Manager (such as YARN, Mesos, or Spark’s standalone manager) to allocate resources across the cluster. The cluster consists of multiple Worker Nodes, each hosting Executors—JVM processes responsible for executing tasks and storing data in memory for caching. Executors handle task execution and report back to the driver. This architecture enables distributed computation, fault tolerance, and scalability across large datasets.

---

### 2. Spark Execution Flow

![Spark Execution Flow](https://github.com/user-attachments/assets/60b674f6-2729-4537-b8e1-e6e776ec8c6a)

This flowchart represents the lifecycle of a Spark job from submission to completion. The process begins when the application is submitted by the driver, which builds a Directed Acyclic Graph (DAG) of transformations. The DAG Scheduler splits this graph into Stages, separated by shuffle boundaries. Each stage is further divided into Tasks, which are the smallest units of execution. The Task Scheduler assigns these tasks to executors on worker nodes. Execution occurs in parallel across the cluster, leveraging data locality for efficiency. The flow emphasizes Spark’s lazy evaluation model, where transformations are only computed upon an action, and highlights fault tolerance through lineage-based recomputation.

---

### 3. Spark Job Execution Internals

![Spark Job Execution Internals](https://github.com/user-attachments/assets/fdfd5567-4ce1-45fc-94ce-e4781700cb1d)

This image dives deeper into Spark’s internal mechanics during job execution. After the DAG is created, the DAG Scheduler determines shuffle dependencies and creates stages. The Task Scheduler then dispatches tasks to executors, considering locality and resource availability. Executors manage in-memory caching, reducing recomputation for iterative algorithms. Shuffle operations involve writing intermediate data to disk and transferring it across nodes, which is a critical performance factor. Spark also implements fault tolerance by tracking RDD lineage, allowing lost partitions to be recomputed. Memory management strategies, such as unified memory for execution and storage, ensure efficient resource utilization.




---

## 🏁 Example

```python
from pyspark.sql import SparkSession

# Step 1: Initialize SparkSession
spark = SparkSession.builder.appName("Example").getOrCreate()

# Step 2: Create an RDD
data = [1, 2, 3, 4, 5]
rdd = spark.sparkContext.parallelize(data)

# Step 3: Transformations
transformed_rdd = rdd.filter(lambda x: x > 2).map(lambda x: x * 2)

# Step 4: Action
result = transformed_rdd.collect()
print("Transformed result:", result)  # Output: [6, 8, 10]

# Step 5: Stop SparkSession
spark.stop()
```

> 💡 **How it works:**  
> 1. SparkSession starts the app  
> 2. RDD created from list  
> 3. Transformations (filter, map) build DAG  
> 4. `collect()` triggers execution  
> 5. Results printed in driver

---

## 🔑 Spark Core Concepts

- **RDD**: Immutable distributed collection of objects
- **DataFrames**: Distributed collections with schema
- **Datasets**: Type-safe structured collection

<p align="center">
  <img width="345" height="146" alt="Spark Core Concepts" src="https://github.com/user-attachments/assets/77d5535a-d824-4925-b77f-6c46b2df00f9" />
</p>

---

## 📖 Resources

- [Apache Spark Docs](https://spark.apache.org/docs/latest/)
- [PySpark API Reference](https://spark.apache.org/docs/latest/api/python/)
- [MLlib Guide](https://spark.apache.org/mllib/)
- [Azure Databricks](https://azure.microsoft.com/en-us/products/databricks/)
- [Google Cloud Dataproc](https://cloud.google.com/dataproc)
- [AWS EMR](https://aws.amazon.com/emr/)

---

<p align="center">
  <em>Made with ❤️ by <a href="https://github.com/rubenjcano">rubenjcano</a></em>
</p>
