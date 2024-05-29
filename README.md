# OpenHDMap

## Key terms and definition 
To understand the GitHub README for the OpenHDMap project, here's a breakdown of key terms and their definitions:

1. **HD Map (High Definition Map)**: Detailed maps that provide highly precise information about roads, lanes, traffic signs, and other relevant features for autonomous driving.

2. **Lidar**: A sensor that measures distances by illuminating the target with laser light and measuring the reflection with a sensor. Used for creating high-resolution maps.

3. **Point Cloud**: A set of data points in space produced by 3D scanners like lidar. Each point represents a location on a surface within the scanned area.

4. **Point Cloud Registration**: The process of aligning multiple point clouds to create a comprehensive model. Also known as stitching.

5. **NDT Mapping (Normal Distributions Transform Mapping)**: A method for mapping using lidar data that builds a probabilistic representation of the environment to align point clouds.

6. **SLAM (Simultaneous Localization and Mapping)**: A process where a device simultaneously builds a map of its surroundings and keeps track of its location within that map.

7. **Autoware**: An open-source software project that provides a complete system for autonomous driving. It includes modules for perception, planning, and control.

8. **LOAM (Lidar Odometry and Mapping)**: An algorithm used for real-time lidar odometry and mapping.

9. **OpenDrive**: A format for describing road networks to support driving simulation.

10. **Sensor Fusion**: Combining data from multiple sensors to produce more accurate, reliable information.

11. **Map Layers**:
    - **Base Layer**: Contains structured map information like lane lines and traffic rules.
    - **Positioning Layer**: Includes original point clouds and features that aid in positioning, such as buildings and traffic signs.
    - **Dynamic Layer**: Holds information about dynamically changing conditions, such as traffic or accidents.

12. **Apollo HD Map**: A high-definition map format used by the Apollo autonomous driving platform.

13. **Map Collection**: The process of gathering data using a vehicle equipped with sensors like lidar, cameras, GPS, and IMU (Inertial Measurement Unit).

14. **Map Labeling**: Annotating the map with additional information such as lane lines, traffic signs, and lights. This can be done manually or automatically.

15. **Map Saving**: Storing the processed and labeled map data in a specific format and structure.

16. **KITTI**: An open dataset widely used in autonomous driving research containing raw data collected by driving a car around a mid-size city.

17. **Interactive SLAM**: A type of SLAM that involves user interaction to improve mapping accuracy.

18. **Autonomous Driving Simulation**: Using software to simulate driving scenarios for testing and development of autonomous driving technologies.

### Structure of the Project
- **data**: Directory where datasets are stored.
- **docs/img**: Directory containing documentation and images.
- **map_format_tool**: Tools for converting map formats.
- **map_label_tool**: Tools for labeling maps.
- **map_production**: Directory for creating high-precision maps.

### Workflow Overview
1. **Map Collection**: Collecting data using a sensor-equipped vehicle.
2. **Map Production**: Generating 3D models from point cloud data using techniques like NDT Mapping.
3. **Map Labeling**: Annotating the 3D map with additional details manually or using sensor fusion.
4. **Map Saving**: Storing the map in formats like OpenDrive, and structuring it into layers for various uses.

### Tools and References
- **Autoware NDT Mapping**: Tool used for creating maps from point cloud data.
- **Map Annotation Tool**: Soon-to-be-released tool for labeling maps.
- **Apollo and Autoware Integration**: Formats provided for compatibility with these platforms.

### Quick Start Guide
- **docs**: Documentation.
- **map_format_tool**: Tools for format conversion.
- **map_label_tool**: Tools for map labeling.
- **map_production**: Tools for creating high-precision maps.

This breakdown should help you understand the key concepts and workflow of the OpenHDMap project.

This is an open source HD map project for autonomous driving. The precision map production process is divided into four parts: **map collection, map production, map labeling, and map saving**. This project mainly uses lidar as a collection sensor to provide the map making process.  
## Overview
The goal is to provide a complete mapping process for autonomous driving systems and simulation.  

If you have any advice, please feel free to contact us.  

## Introduce  
Let ’s start with how to make an HD map. The HD map making process can be divided into 4 steps. 
1. Map collection  
2. Map production  
3. Map labeling  
4. Map saving  

Next we introduce these four processes respectively.  
![HDmap_pipeline](docs/img/hdmap_pipeline.jpg)  

#### 1. Map collection
First, we use a map collection car equipped with lidar, camera, GPS and IMU to collect maps. The data currently used is an open source dataset. We plan to provide recommended hardware and toy cars later.  

You can use the collected data set or equip your own sensors to collect data.    
* Collected dataset: KITTI  
* Recommended equipment: todo (If you have good suggestions, welcome to recommend)  

#### 2. Map production
Then, We need to generate street and building models based on the collected point cloud data. The production process is the stitching of point clouds. This process is often referred to as point cloud registration. After stitching the point clouds frame by frame, a three-dimensional model of the entire street is obtained. You scanned the whole earth with lidar. it looks really cool, right?  

There are currently two methods to generate point cloud maps, one is through NDT Mapping, and the other is offline SLAM mapping. At present, we choose to use NDT Mapping. Here we refer to the implementation of autoware.  

There are several SLAM mapping methods to refer to:  
* LOAM
* Cartographer
* hdl_graph_slam
* blam
* A-LOAM
* LeGO-LOAM
* LIO-mapping
* interactive_slam

> We will use the above method to build the map later.

#### 3. Map labeling
After you have completed the 3D model of the map, this part of the map is called a point cloud map. To get lane line, traffic lights and traffic sign information, we also need to label the map. At present, labeling is mainly done manually. We will provide labeling tools. The labeled map is called HD map.  

The two most important questions in map labels are as follows.  
* **Sensor fusion** - Because the resolution of the point cloud is not high, the purpose of sensor fusion is to map the pictures of the camera to the point cloud to improve the resolution of lane lines and traffic signs.  
* **Automatic labeling** - Automatic labeling capability for large-scale map production and maintenance.  

At present, we adopt the method of manual annotation, and we will gradually solve the above two problems later.  


#### 4. Map saving
The preservation of HD maps is divided into two parts, one is the format of the map, and the other is the layers of the map.
* Map format   
The format of the map is currently opendrive. In short, the project hopes to have a unified labeling format.  

- Map layers    
Map is divided into 3 layers: base layer, positioning layer and dynamic layer.
    - Base layer. The basic layer is the structured information of the map generated by the above annotations, including lane lines, traffic rule information, etc.  
    - Positioning layer. The positioning layer is some original point clouds that are helpful for autonomous driving positioning, such as telephone poles, traffic signs, buildings, etc.  
    - Dynamic layer. The dynamic layer publishes some dynamically changing information, such as traffic conditions, traffic accidents ahead, etc.

We currently use Apollo HD map format to work with simulator to verify HD maps.  

## Quick start

#### Introduction
```
.
├── docs             // documents
├── map_format_tool  // Implement map format conversion function
├── map_label_tool   // Map labeling tool
├── map_production   // Create high-precision map which use to label
```

#### 1. Map production
We use "Autoware NDT Mapping" to build a small map from open dataset. The code is reference in "core_perception/lidar_localizer/nodes/ndt_mapping" [Link](https://gitlab.com/autowarefoundation/autoware.ai/core_perception). The code is now in dir "map_production".  

#### 2. Map labeling
We will soon release a map annotation tool, which will in "map_label_tool" dir.  

#### 3. Map format
After labelling semantic maps, we provide multiple map formats to different open source projects like Apollo and autoware.  

#### 4. End-to-end pipeline


## Examples  

## Benchmark  
Benchmark of the process:  
* accurate
* time

## Reference
[vector tiles](https://docs.mapbox.com/vector-tiles/reference/)  
[awesome-vector-tiles](https://github.com/mapbox/awesome-vector-tiles)  
[paperjs](http://paperjs.org/examples/hit-testing/)  
[best-javascript-drawing-libraries](https://www.slant.co/topics/28/~best-javascript-drawing-libraries)  

# Deployment on a K8s based MEC
Deploying the OpenHDMap system on Multi-access Edge Computing (MEC) with Kubernetes (K8s) clusters and 5G connectivity involves several components and steps. Here’s a detailed approach:

### Components and Architecture
1. **MEC Hosts**: Each MEC host serves a specific geographical region.
2. **Kubernetes Clusters**: Each MEC host runs a K8s cluster to manage containerized applications.
3. **5G Connectivity and UPF (User Plane Function)**: Ensures fast and reliable communication between vehicles and MEC hosts.
4. **HDMap Services**: Consist of different microservices for map collection, production, labeling, and saving.
5. **Point Cloud Data Exchange**: Mechanism for sharing parts of point clouds between different HDMap instances.

### Deployment Steps

#### 1. Kubernetes Cluster Setup
- Ensure each MEC host has a Kubernetes cluster installed and properly configured.
- Set up networking and storage for the K8s clusters.
- Deploy necessary Kubernetes controllers and services for managing workloads.

#### 2. Service Deployment on Kubernetes
- **Map Collection Service**: Deploy microservices that handle data collection from vehicles. These services will receive raw sensor data (e.g., lidar, GPS) over 5G.
- **Map Production Service**: Deploy services that perform point cloud registration and create 3D models from collected data.
- **Map Labeling Service**: Deploy tools for manual and automated map labeling.
- **Map Saving Service**: Deploy services that store and manage HD map data, ensuring it’s available in required formats and structured into layers.

#### 3. Point Cloud Data Exchange
- **Inter-MEC Communication**: Use Kubernetes services and network policies to enable communication between MEC hosts.
- **Message Brokers/Queues**: Use systems like Kafka or RabbitMQ to handle the exchange of point cloud data between different MEC regions. This ensures reliability and scalability.
- **Data Consistency**: Implement mechanisms for ensuring data consistency and resolving conflicts when point clouds are exchanged.

#### 4. Edge Node Configuration
- **Vehicle Connectivity**: Configure vehicles to connect to the nearest MEC host via 5G.
- **UPF Integration**: Integrate User Plane Functions (UPF) to manage data traffic efficiently between vehicles and MEC hosts.

### Collaborative HDMap Workflow

1. **Map Collection**:
   - Vehicles collect data using sensors and upload it to the nearest MEC host via 5G.
   - The collected data is stored temporarily in the K8s cluster.

2. **Map Production**:
   - The point cloud data is processed to generate 3D street and building models.
   - Use NDT Mapping or other SLAM methods as required.

3. **Map Labeling**:
   - Label the processed maps with lane lines, traffic signs, and other relevant features.
   - Store labeled data in the appropriate map layers.

4. **Map Saving and Exchange**:
   - Save the HD maps in standardized formats like OpenDrive.
   - Use Kafka/RabbitMQ to publish point cloud data to other MEC hosts.
   - Subscribe to point cloud data from adjacent regions to ensure comprehensive coverage.

### Example Kubernetes Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hdmap-collection
  namespace: hdmap
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hdmap-collection
  template:
    metadata:
      labels:
        app: hdmap-collection
    spec:
      containers:
      - name: collection
        image: hdmap/collection:latest
        ports:
        - containerPort: 8080
        env:
        - name: MEC_HOST
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
---
apiVersion: v1
kind: Service
metadata:
  name: hdmap-collection
  namespace: hdmap
spec:
  selector:
    app: hdmap-collection
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hdmap-production
  namespace: hdmap
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hdmap-production
  template:
    metadata:
      labels:
        app: hdmap-production
    spec:
      containers:
      - name: production
        image: hdmap/production:latest
        ports:
        - containerPort: 8081
---
apiVersion: v1
kind: Service
metadata:
  name: hdmap-production
  namespace: hdmap
spec:
  selector:
    app: hdmap-production
  ports:
  - protocol: TCP
    port: 81
    targetPort: 8081
```

### Additional Considerations

1. **Security**: Ensure secure communication channels (e.g., TLS) between vehicles and MEC hosts, and between MEC hosts themselves.
2. **Scalability**: Use Kubernetes Horizontal Pod Autoscaler (HPA) to scale services based on load.
3. **Monitoring and Logging**: Implement logging and monitoring (e.g., Prometheus, Grafana) for real-time insights and debugging.
4. **Data Privacy**: Ensure compliance with data privacy regulations, especially when handling location and sensor data from vehicles.

This approach ensures that the OpenHDMap system can efficiently manage HD map production and sharing in a distributed and collaborative manner across multiple MEC hosts using Kubernetes and 5G.

# Pytm Threat model
### Introduction to PyTM

PyTM is a threat modeling tool that helps security professionals and developers create Data Flow Diagrams (DFD) and identify potential security threats using the STRIDE methodology. PyTM allows users to define elements like processes, data stores, data flows, and boundaries programmatically, and then automatically generates a DFD and associated threat analysis.

### Key Concepts

- **Data Flow Diagram (DFD)**: A graphical representation of data flows through a system, showing processes, data stores, data flows, and external entities.
- **STRIDE**: A threat modeling framework that identifies six categories of threats:
  - **Spoofing**: Impersonating something or someone else.
  - **Tampering**: Modifying data or code.
  - **Repudiation**: Denying an action.
  - **Information Disclosure**: Exposing information to unauthorized entities.
  - **Denial of Service (DoS)**: Making a system unavailable.
  - **Elevation of Privilege**: Gaining unauthorized access to resources.

### PyTM Example for OpenHDMap Deployment

Below is an example PyTM script for the described OpenHDMap deployment on MEC with Kubernetes, including generating the DFD and identifying STRIDE threats.

```python
from pytm import TM, Server, Process, Dataflow, Boundary, Data, Actor, Store

# Define the Threat Model
tm = TM("OpenHDMap Deployment on MEC")

# Define boundaries
Internet = Boundary("Internet")
KubernetesCluster = Boundary("Kubernetes Cluster")
MEC = Boundary("MEC")
VehicleNetwork = Boundary("Vehicle Network")

# Define elements
car = Actor("Car")
vehicle_sensors = Process("Vehicle Sensors")
lidar_data = Data("Lidar Data")
gps_data = Data("GPS Data")
camera_data = Data("Camera Data")
imu_data = Data("IMU Data")

mec_host = Server("MEC Host")
k8s_master = Server("K8s Master Node")
k8s_worker = Server("K8s Worker Node")

data_collection_service = Process("Data Collection Service")
map_production_service = Process("Map Production Service")
map_labeling_service = Process("Map Labeling Service")
map_saving_service = Process("Map Saving Service")
point_cloud_data = Data("Point Cloud Data")
hd_map = Store("HD Map")

inter_mec_comm = Dataflow("Inter-MEC Communication", mec_host, mec_host, data="Point Cloud Data")

# Define data flows
df1 = Dataflow("Sensor Data to MEC", car, vehicle_sensors, data=[lidar_data, gps_data, camera_data, imu_data])
df2 = Dataflow("Sensor Data to Collection Service", vehicle_sensors, data_collection_service)
df3 = Dataflow("Collected Data to Production Service", data_collection_service, map_production_service, data=point_cloud_data)
df4 = Dataflow("Processed Data to Labeling Service", map_production_service, map_labeling_service)
df5 = Dataflow("Labeled Data to Saving Service", map_labeling_service, map_saving_service)
df6 = Dataflow("HD Map Storage", map_saving_service, hd_map)

# Define trust boundaries
df1.boundary = VehicleNetwork
df2.boundary = MEC
df3.boundary = KubernetesCluster
df4.boundary = KubernetesCluster
df5.boundary = KubernetesCluster
df6.boundary = KubernetesCluster
inter_mec_comm.boundary = Internet

# Set threat model properties
tm.process()

# Print the DFD diagram
tm.dataflows()

# Generate STRIDE threats
tm.threats()
```

### Explanation of Elements

- **Boundaries**: Define different zones such as Internet, Kubernetes Cluster, MEC, and Vehicle Network.
- **Actors and Processes**: Represent different components such as the car, sensors, MEC host, Kubernetes master and worker nodes, and various services.
- **Data Flows**: Show the movement of data between components, such as sensor data from cars to the MEC, processing and labeling services, and HD map storage.
- **Trust Boundaries**: Indicate where data flows cross security boundaries, which is critical for identifying potential threats.

### STRIDE Threat Analysis

Using the above PyTM script, here are the potential STRIDE threats identified:

1. **Spoofing**
   - Impersonation of vehicles or sensors to inject false data into the system.

2. **Tampering**
   - Modification of sensor data in transit (e.g., lidar, GPS).
   - Unauthorized alteration of point cloud data during inter-MEC communication.

3. **Repudiation**
   - Denying the submission of incorrect or malicious data by a vehicle.

4. **Information Disclosure**
   - Leakage of sensitive data such as vehicle location and sensor data during transmission.
   - Unauthorized access to HD maps stored in the Kubernetes cluster.

5. **Denial of Service (DoS)**
   - Overloading the data collection service to disrupt map production.
   - Attacks on Kubernetes infrastructure to render MEC hosts unresponsive.

6. **Elevation of Privilege**
   - Gaining elevated permissions within the Kubernetes cluster to alter map data or disrupt services.

### Mitigation Strategies

To address these threats, consider implementing the following mitigations:

- **Authentication and Authorization**: Ensure strong authentication mechanisms for vehicles and sensors. Use role-based access control (RBAC) within Kubernetes.
- **Data Encryption**: Encrypt data in transit and at rest to protect against tampering and information disclosure.
- **Integrity Checks**: Use checksums or digital signatures to verify the integrity of data flows.
- **Monitoring and Logging**: Implement comprehensive monitoring and logging to detect and respond to suspicious activities.
- **Rate Limiting and Load Balancing**: Apply rate limiting to prevent DoS attacks and use load balancers to distribute the load effectively.
- **Secure Inter-MEC Communication**: Use secure channels (e.g., TLS) for data exchange between MEC hosts.

This approach using PyTM and STRIDE helps create a robust security framework for deploying the OpenHDMap system on MEC with Kubernetes, ensuring data integrity, confidentiality, and availability.

