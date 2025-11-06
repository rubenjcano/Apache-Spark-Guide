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


This repository provides a comprehensive guide to **Apache Spark** for aspiring Data Engineers. It covers core concepts, hands-on examples, and best practices for working with Spark in distributed environments.

---

# Table of Contents

1. [Introduction](https://github.com/rubenjcano/Apache-Spark-Guide#Introduction)

2. [Installation & Setup](https://github.com/rubenjcano/Apache-Spark-Guide#Installation-&-Setup)

3. [Spark Core Concepts]

4. [PySpark]

5. [Spark SQL]

6. [Spark Streaming]

7. [Machine Learning with MLib]

8. [Integration with Cloud Platforms (Azure, GCP, AWS)]

9. [Sample Projects]

10. [Resources]

---

## ✨ Introduction

### What is Apache Spark?

**Apache Spark** is an **open-source**, distributed computing framework designed for **fast** and **scalable data processing**. It enables organizations to **analyze massive datasets efficiently** by scaling workloads across multiple nodes.

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
   Spark’s core abstraction is the **RDD (Resilient Distributed Dataset)**—an immutable and distributed data collection. Users define transformations (like `filter` or `map`) that are _lazy_ and only executed upon an action.
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


#### 🏁 Example

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

#### 1. Spark Architecture

![Spark Architecture](https://github.com/user-attachments/assets/6b3e6edd-b301-44b2-b216-87c23ebe2434)

This diagram illustrates the core components of Apache Spark’s architecture and their interactions. At the center is the Driver Program, which runs the SparkContext and coordinates the entire application across a Spark cluster.

---

#### 2. Spark Execution Flow

![Spark Execution Flow](https://github.com/user-attachments/assets/60b674f6-2729-4537-b8e1-e6e776ec8c6a)

This flowchart represents the lifecycle of a Spark job from submission to completion. The process begins when the application is submitted by the driver, which builds a Directed Acyclic Graph (DAG) of execution.

---

#### 3. Spark Job Execution Internals

![Spark Job Execution Internals](https://github.com/user-attachments/assets/fdfd5567-4ce1-45fc-94ce-e4781700cb1d)

This image dives deeper into Spark’s internal mechanics during job execution. After the DAG is created, the DAG Scheduler determines shuffle dependencies and creates stages. The Task Scheduler then schedules tasks on executors.

---

## ⚙️ Installation & Setup

> **Get Spark Up and Running!**  
>
> This section shows you step-by-step how to install Apache Spark, set up PySpark, and prepare your environment—either locally or in the cloud.  

### 1. Choose Your Environment

You can use Spark locally for development or deploy it on the cloud for large workloads.  
- **Local:** Quick prototyping, learning, small data.
- **Cloud:** Scalable, distributed processing with Spark clusters.

### 2. Install Java (Required for Spark)

Apache Spark runs on the JVM, so you need Java 8 or higher.

- **Linux/macOS:**  
  ```bash
  sudo apt update
  sudo apt install openjdk-11-jdk
  java -version
  ```
- **Windows:**  
  Download and install [Adoptium JDK](https://adoptium.net/).

### 3. Install Apache Spark (Standalone)

#### Option A: Download & Extract

1. Go to the [Apache Spark Downloads page](https://spark.apache.org/downloads.html).
2. Download a pre-built Spark release (recommended: with Hadoop included).
3. Extract it:
   ```bash
   tar -xzf spark-*.tgz
   cd spark-*
   ```

#### Option B: Install via PySpark (Python users, easiest for local)

```bash
pip install pyspark
```

> This sets up Spark in your Python environment using the PySpark API.

### 4. Set Environment Variables

Ensure the following environment variables are set, especially for Spark’s shell:

```bash
export SPARK_HOME=/path/to/spark
export PATH=$SPARK_HOME/bin:$PATH
```

### 5. Verify Installation

- **Spark shell:**  
  ```bash
  $SPARK_HOME/bin/spark-shell
  ```
- **PySpark shell:**  
  ```bash
  pyspark
  ```
  Should open an interactive shell with Spark context initialized.

### 6. Run a Simple Example (Python)

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Example").getOrCreate()
rdd = spark.sparkContext.parallelize([1, 2, 3, 4, 5])
result = rdd.filter(lambda x: x > 2).map(lambda x: x * 2).collect()
print(result)
spark.stop()
```

If you see the transformed result printed, your setup is working!

---

### 7. Cloud Setup References

If you wish to use Apache Spark in the cloud, check out these managed services and guides:

- [Databricks on Azure](https://learn.microsoft.com/en-us/azure/databricks/)
- [Google Cloud Dataproc](https://cloud.google.com/dataproc/docs/quickstarts)
- [AWS EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-gs.html)

You can launch clusters, connect via notebooks (Jupyter), and run Spark jobs at scale.

---

### 8. Helpful Links

- [Apache Spark Quick Start Guide](https://spark.apache.org/docs/latest/quick-start.html)
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [Spark on Kubernetes](https://spark.apache.org/docs/latest/running-on-kubernetes.html)

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

## 🧩 Spark Core Concepts

<p align="center">
  <img width="345" height="146" alt="Spark Core Concepts" src="https://github.com/user-attachments/assets/77d5535a-d824-4925-b77f-6c46b2df00f9" />
</p>

> **Understanding Spark's Foundation**
>
> Here you’ll explore Spark’s core abstractions—RDDs, DataFrames, and Datasets. We’ll break down what makes Spark’s distributed computing model so powerful, why transformations are lazy, and...

---

## 🐍 PySpark

> **Harnessing Spark with Python**
>
> PySpark brings Spark’s power to Python! Discover how to create RDDs, DataFrames, and run SQL queries right in your favorite language. With hands-on examples, you’ll see how to process data, transform and analyze it at scale.

---

## 🔷 Spark SQL

> **Structured Data Processing Made Easy**
>
> With Spark SQL, you can query structured data using both SQL and DataFrame APIs. Learn how Spark handles schema inference, integrates with data sources (like Parquet or Hive), and accelerates analytic workloads.

---

## 🌊 Spark Streaming

> **Real-Time Data, Real-World Solutions**
>
> Spark Streaming focuses on processing live data streams—logs, events, sensor data, and more. Understand the difference between batch and streaming jobs, build pipelines that react to incoming data instantly.

---

## 🤖 Machine Learning with MLlib

> **Data Science at Scale**
>
> MLlib makes machine learning scalable and simple within Spark. Explore algorithms for classification, regression, clustering, and recommendations—all distributed for big data. Learn the steps to build, train and deploy models in Spark.

---

## ☁️ Integration with Cloud Platforms (Azure, GCP, AWS)

> **Deploying Spark Anywhere**
>
> This section shows how to run Spark jobs in the cloud: Azure, Google Cloud, and AWS. Follow end-to-end guides on configuring clusters, connecting to cloud data storage, and executing distributed workloads.

---

## 🗂️ Sample Projects

> **Hands-On Spark Applications**
>
> Dive into practical sample projects demonstrating how Spark is used in real data engineering scenarios. From ETL jobs to analytics dashboards, these examples feature step-by-step guides to help you build your own data solutions.

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
