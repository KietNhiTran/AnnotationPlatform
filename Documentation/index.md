# AnnotationPlatform

# Table of Content
* System Conceptual Architect
* System Entity Relationship Diagram (ERD)
* Systen Bahevior 

# System Conceptual Architect
## System definition
Annotation system is an internal tool for worker to label certain activites and subjects captured in recorded video. The labeled data later will be consumsed by
* **Internal development team** for correctness validating and improvement seeking. 
* **Data scientist** to build model by getting insight from data that enriches orther products within the company with inllegent solution (compliant system, evidence system, intellegent activities recognition).

The system shell be able to extend to collect different required data source to support solution requirement of data scientists. 

Below is conceptual architect of the system.

## Conceptual design

![Annotation Platform Conceptual Architect - Block Diagram](images/Annotation_System.jpg)

The system consists of front end components and serveral busines logic services (or components) which are in reality implemented as micro-services in cloud deployment platform (Microsoft Asuze). Except the front end component, these services have it own functionality and storage.

In general, these services are independent and they communicates with each other mainly via a queue. Except the front end components, they communicate with backend business logic service via routed HTTPs REST call.

Below is high level description for each services. It is presented based on the flow of the data. First described component is where the data is collected ==> then data is preprocessed ==> stored ==> then annotation workflow assignment is generated ==> data is annotated by worker ==> finally, annotated data is validated by development team ==> then consumsed by data science researcher.

### Hoover
This is a data collector service which scans video / audio data from **Evidence.com** periodically. The schedule is configurable. 

The collected data is stored as binary (blob) format in **Microsoft Asure blob storage**. Each video is stored as an **asset** and download-able **blob content** file. Once the asset is stored, new preprocessing item is put in communication queue for preprocessing. 

### Preprocessing
This services preprocesses the collected data (currently from **Hoover**). The process gets triggerred by new arrived asset raised in communicate queue (ActiveMQ - **preprocessing queue**). 

The processing activites includes:
* Automatically roughtly classifying activity (**Hit**) into correspondent type (**HitType**)
* Automatically reducing duplicated, similar video frame to minimize required annotating effort.

# System Entity Relationship Diagram (ERD)

![Annotation Platform Value Range Structure - ERD Diagram](images/Annotation_ERD.jpg)
