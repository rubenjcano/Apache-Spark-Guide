<h1 align="center">
  <img width="230" height="120" alt="image" src="https://github.com/user-attachments/assets/c50f15bb-818c-4f87-a577-bee7539aeffa" />
    <br />
  Apache Spark Guide for Data Engineers
</h1>

This repository provides a comprehensive guide to Apache Spark for aspiring Data Engineers. It covers core concepts, hands-on examples, and best practices for working with Spark in distributed data processing environments.

<img width="1266" height="460" alt="image" src="https://github.com/user-attachments/assets/e883f51f-8f48-4124-b976-a2edc6067a94" />

# Table of Contents

1. [Introduction](https://github.com/rubenjcano/Apache-Spark-Guide#Introduction)

2. [Installation Set up]

3. [Spark Core Concepts]

4. [PySpark]

5. [Spark SQL]

6. [Spark Streaming]

7. [Machine Learning with MLib]

8. [Integration with Cloud Platforms (Azure, GCP, AWS)]

9. [Sample Projects]

10. [Resources]

# Introduction

## What is Apache Spark?

### **Apache Spark Overview**

**Apache Spark** is an **open-source**, **distributed computing framework** designed for **fast** and **scalable data processing**. It enables organizations to **analyze massive datasets efficiently** by leveraging **in-memory computation** and **parallel processing** across **clusters**. Unlike traditional systems such as **Hadoop MapReduce**, which rely heavily on **disk I/O**, Spark stores **intermediate data in memory**, making it **up to 100 times faster** for certain workloads.

Spark provides a **unified platform** for diverse **data processing tasks**, including **batch processing**, **real-time streaming**, **interactive queries**, and **machine learning**. Its **architecture** supports multiple **languages**—such as **Python**, **Scala**, **Java**, and **R**—through **high-level APIs**, making it accessible to **developers** and **data engineers** alike.

With **built-in libraries** like **Spark SQL**, **MLlib** for **machine learning**, **GraphX** for **graph processing**, and **Structured Streaming** for **real-time analytics**, Spark simplifies **complex workflows** into a **single ecosystem**. Whether deployed on a **single machine** or scaled across **thousands of nodes** in a **cluster**, Spark delivers **speed**, **flexibility**, and **fault tolerance** for **modern big data applications**.

## How Apache Spark works?

Apache Spark is a distributed computing framework designed for large-scale data processing. A Spark application begins when the user submits code through either a SparkContext or a SparkSession. While SparkContext is the original entry point for working with RDDs, SparkSession is the newer, unified interface that also supports structured data through DataFrames and SQL.

The driver program orchestrates the execution by communicating with a cluster manager (like YARN or Kubernetes), which allocates resources and launches executors on worker nodes. These executors are responsible for running tasks and storing data during computation.
Spark’s core abstraction is the Resilient Distributed Dataset (RDD)—an immutable, distributed collection of data that can be processed in parallel. Users apply transformations (like filtering or mapping) to RDDs, which are lazy and build a logical execution plan. When an action (like collecting results) is triggered, Spark constructs a Directed Acyclic Graph (DAG) to represent the computation.

The DAG is divided into stages based on data shuffles, and each stage consists of tasks that are distributed to executors. Executors run these tasks in parallel, cache intermediate results if needed, and return final outputs to the driver.
Spark ensures fault tolerance by tracking the lineage of RDDs, allowing lost data to be recomputed. It also includes performance optimizations like the Tungsten engine and Catalyst optimizer, and supports high-level libraries for SQL, streaming, machine learning, and graph processing.

In essence, Spark coordinates distributed data processing through a driver, manages resources via a cluster manager, executes tasks on executors, and delivers scalable, fault-tolerant results efficiently.

<p align="center">
  <img width="1024" height="551" alt="image" src="https://github.com/user-attachments/assets/6b3e6edd-b301-44b2-b216-87c23ebe2434" />
</p>

This diagram shows the basic Spark architecture in a very simplified way. The Master Node runs the Driver Program, which contains the SparkContext—the entry point for Spark operations. The driver communicates with the Cluster Manager, which allocates resources and launches executors on Worker Nodes. Each worker node executes assigned tasks in parallel and can cache data in memory for faster processing. In short, the driver plans and coordinates, the cluster manager handles resource allocation, and worker nodes perform the actual computation.

<p align="center">
  <img width="1114" height="344" alt="image" src="https://github.com/user-attachments/assets/60b674f6-2729-4537-b8e1-e6e776ec8c6a" />
</p>

This diagram shows the execution flow of a Spark application. The process starts with the User Program, where transformations like map, filter, and reduceByKey are defined on RDDs. These operations are managed by the SparkContext, which builds a Directed Acyclic Graph (DAG) representing the computation. The DAG Scheduler splits the DAG into stages and tasks, which are then sent to the Cluster Manager. The cluster manager allocates resources and launches executors on worker nodes. Executors run tasks in parallel and use cache to store intermediate data for faster processing. In short, Spark converts user code into a DAG, schedules tasks, and executes them across distributed workers efficiently.

<p align="center">
  <img width="1029" height="721" alt="image" src="https://github.com/user-attachments/assets/fdfd5567-4ce1-45fc-94ce-e4781700cb1d" />
</p>

This diagram illustrates the internals of job execution in Apache Spark. When a user submits code, the Driver Program (with SparkSession or SparkContext) interprets transformations and actions. Once an action is triggered, SparkContext creates a job, builds a DAG (Directed Acyclic Graph) of transformations, and splits it into stages. The DAG Scheduler organizes these stages, and the Task Scheduler converts them into tasks. The Cluster Manager then allocates resources and launches executors on worker nodes, which run tasks in parallel and can cache intermediate data. Finally, executors return results to the driver, completing the distributed computation efficiently.

## Example

```python
from pyspark.sql import SparkSession

# Step 1: Initialize SparkSession (entry point for Spark)
spark = SparkSession.builder.appName("Example").getOrCreate()

# Step 2: Create a simple dataset (RDD) from a Python list
# This simulates distributed data without loading from external sources
data = [1, 2, 3, 4, 5]
rdd = spark.sparkContext.parallelize(data)

# Step 3: Apply transformations
# Filter numbers greater than 2 and then multiply them by 2
transformed_rdd = rdd.filter(lambda x: x > 2).map(lambda x: x * 2)

# Step 4: Trigger an action
# collect() brings the processed data back to the driver as a Python list
result = transformed_rdd.collect()

# Step 5: Print the result
# Expected output: [6, 8, 10]
print("Transformed result:", result)

# Step 6: Stop SparkSession
spark.stop()
```
#### What happens here:

1. SparkSession starts the application.
2. An RDD is created from a local list.
3. Transformations (filter, map) build a DAG but don’t execute yet.
4. collect() triggers execution, sending tasks to executors.
5. Results are returned to the driver and printed.


# Spark Core Concepts
RDD
DataFrames
Datasets


<img width="345" height="146" alt="image" src="https://github.com/user-attachments/assets/77d5535a-d824-4925-b77f-6c46b2df00f9" />
