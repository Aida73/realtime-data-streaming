
# Realtime data streaming pipeline

This end-to-end data engineering pipeline aims to fetch, transform, and store data from the randomuser.me API into a database. 

## Architecture 
Here's a summary of the main steps and technologies used:

- **Data Retrieval:** An Airflow DAG is used to periodically fetch data from the randomuser.me API at approximately 1-minute intervals.
- **Data Streaming with Kafka:** The retrieved data is streamed to a Kafka broker in a specific topic. Apache Zookeeper is used for internal management of the Kafka cluster, including broker coordination and partition management.
- **Visualization with Control Center:** Kafka data is visualized in a Control Center providing a user interface to monitor incoming and outgoing messages from Kafka topics. An Schema Registry is also used to store and retrieve data schemas in data streams, contributing to the robustness and compatibility of distributed Kafka applications.
- **Processing with Spark:** Data is then streamed into a Spark cluster, using a master-workers architecture. The Spark job involves transferring data from the Kafka topic to a Cassandra database.
- **Orchestration with Docker:** All components of the architecture are launched in Docker containers, orchestrated using a single Docker-compose file.

## Launch the project locally

```bash
    cd endToEndDeProject
    docker-compose up -d --build
```

- Go to localhost:8080 to launch the dag in Airflow
- After submit the job with this command:
    ```bash 
    spark-submit --master spark://localhost:7077 --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.1.2 
    com.datastax.spark:spark-cassandra-connector_2.12:3.1.0 spark_stream.py
    ```

- You can visit the control center page (link available in the Docker Desktop Dashboard) to visualize the fetched data from the API in the Topic users_created.
- Enter into the cassandra container to visualize data fetched from kafka.

## Technical Stacks Used: 
Airflow, Apache Spark, Apache Zookeeper, Apache Kafka, Python, Cassandra, Docker.
