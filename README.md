# Distributed Key-Value Storage System Documentation

## Introduction

This project aims to create a distributed key-value storage system with sharding and replication capabilities. The system is designed to be scalable, fault-tolerant, and efficient in handling key-value data across multiple nodes in a distributed environment.

## Components and Functionality

### KVStorage Servers

The KVStorage servers are responsible for storing key-value data. The following operations are supported:

1. **Put(key, value):** Saves the given value into the specified key. If the key already exists, its value is overwritten.
2. **Get(key):** Retrieves the value associated with the given key. If the key does not exist, it returns null (None in Python).
3. **Append(key, value):** Concatenates the specified value to the leftmost end of the value associated with the key. If the key does not exist, it creates a new entry with the provided value.
4. **R_pop(key):** Returns the rightmost character of the value associated with the key and deletes it from the stored value. If the last character is popped, the value becomes an empty string ("", length 0). If the key does not exist, it returns null (None in Python).
5. **L_pop(key):** Returns the leftmost character of the value associated with the key and deletes it from the stored value. If the last character is popped, the value becomes an empty string ("", length 0). If the key does not exist, it returns null (None in Python).

### Sharding

Data sharding is implemented to divide the data into equal shards distributed across multiple storage nodes. The ShardMaster server is responsible for shard assignment and redistribution. The shard master provides the following operations:

1. **Join(server):** A storage server joins the system. The shard master stores the new server's address and redistributes the keys based on the new number of servers.
2. **Leave(server):** A storage server leaves the system. The shard master removes the server's address from the servers list and redistributes the keys based on the new number of servers.
3. **Query(key):** Asks the shard master about the server that holds the provided key. Returns the corresponding server address.

### Replication

Replication is used to maintain multiple copies of the same data on different servers, ensuring fault tolerance and data availability. The system implements the Primary/Secondary (P/S) replication protocol with the following consistency levels:

1. **Strong Consistency:** All servers in a replica group are updated synchronously, ensuring they always have the same data. Read requests return the most recent value of the data.
2. **Eventual Consistency:** Replicas of the data may be inconsistent during a certain period but eventually converge to hold identical data. Some secondary replicas are updated at each write, while others receive updates periodically.

## Client Interaction

Clients interact with the system as follows:

1. The client queries the ShardMaster server to determine the server holding the shard corresponding to the requested key.
2. Depending on the operation type (read or write), the ShardMaster server responds with the replica master or any secondary replica of the shard.
3. The client directly performs the storage request to the specified server.

## Evaluation and Testing

The system is evaluated using throughput and consistency measurements for different configurations. Test cases are designed to evaluate system performance, scalability, and data consistency in various scenarios. The implementation handles edge cases and potential errors to ensure system correctness and reliability.

## Conclusion

The distributed key-value storage system with sharding and replication provides a robust and scalable solution for managing key-value data in a distributed environment. The implementation demonstrates high availability, fault tolerance, and efficient data access, making it suitable for real-world distributed systems. The project serves as a significant step in understanding and implementing distributed systems effectively.

# Installation
· Linux
```bash
python3 -m pip install -r requirements.txt
python3 -m grpc_tools.protoc --proto_path=. --grpc_python_out=. --pyi_out=. --python_out=. ./KVStore/protos/*.proto
python3 -m pip install -e .
```
· Windows
```bash
python3 -m pip install -r requirements.txt
python3 -m grpc_tools.protoc --proto_path=. --grpc_python_out=. --pyi_out=. --python_out=. ./KVStore/protos/*.proto
python3 -m pip install -e .
```

# Evaluation
## First subtask (simple KV storage)
```bash
python3 eval/single_node_storage.py
```

## Second subtask (sharded KV storage)
```bash
python3 eval/sharded.py
```

## Third subtask (sharded KV storage with replica groups)
```bash
python3 eval/replicas.py
```

