# INT-based IoT anomaly detection with 1D-CNN model



The purpose of this GitHub project is to provide a solution for IoT anomaly detection in Software-Defined Networking (SDN) environments. The project addresses the security challenges associated with IoT technologies by integrating both the control and data plane capabilities. 
The primary objectives of the project include:


 **1. Integration of Control and Data Plane:**
 
- Objective: Integrate both the control and data plane to enhance IoT anomaly detection in SDN environments.
- Approach: Utilize real-time network telemetry data(INT) from the data plane and offer a an anomaly detection solution based on the control plane, utilizing this INT data.


**2. Building Anomaly Detection Model:**

- Objective: Develop and compare various anomaly detection models to identify the most effective approach for INT data.
- Approach: Build different models using the collected telemetry and benchmark data. Determine that the one-dimensional Convolutional Neural Network (1D CNN) model performs the best among the tested models.

# Project Features
**1. Data Collection:**

- Collect real-time network telemetry data from our simulated SDN network in normal and DDoS attack condition.
- Preprocessing of collected data 

**2.Model Development:**

- Build and implement different anomaly detection models using the collected data and benchmark datasets. 
- Determine the 1D CNN model as the most effective for IoT anomaly detection.

**3. Performance Evaluation:**

- Evaluate the 1D CNN model's performance using metrics such as accuracy, F1 score, and Matthews correlation coefficient (MCC).
- Compare the proposed solution with existing studies, demonstrating its superiority or comparable performance.

## Publications

The result of this work was presented and discussed at the IEEE 16th International Scientific Conference on Informatics (Informatics 2022) conference and published in IEEE Informatics 2022 conference proceeding [1]. Furthermore, I developed
an extended article for publication in Acta Electrotechnica et Informatica, providing a more comprehensive and detailed account of my research findings [2]. If you need more detailed information, please access the publications. 


- [1] Gereltsetseg Altangerel, Máté Tejfel, and Enkhtur Tsogbaatar. A 1D-CNN based model for IoT anomaly detection using int data. In 2022 IEEE 16th International Scientific Conference on Informatics (Informatics), pages 106–113, 2022. DOI: 

- [2] Gereltsetseg Altangerel, Máté Tejfel, and Enkhtur Tsogbaatar. IoT anomaly detection with 1d cnn using p4 capabilities. Acta Electrotechnica et Informatica, 23(2):3–12, 2023. DOI: 




## Getting Started

### Building testbed SDN network for collecting INT data. 

When creating a virtual SDN network testbed incorporating INT (In-band Network Telemetry), we initially referred to the guidelines provided at the following link: https://wiki.onosproject.org/display/ONOS/In-band+Network+Telemetry+%28INT%29+with+ONOS+and+P4

However, due to the deprecation of this guideline and the unavailability of certain package versions online, we opted to create our own guideline based on the information provided in the deprecated guideline. You can find our customized guideline in the repository under the file named Building_SDN_testbed.md.



### Running proposed 1D-CNN model with testbed dataset

Our proposed model is stored in the file named model.ipynb within this repository. Please execute this file using our designated testbed dataset titled testbed.csv, also available in this repository.

## If you use this repository in your research, please cite:


@INPROCEEDINGS{10083469,

  author={Altangerel, Gereltsetseg and Tejfel, Máté and Tsogbaatar, Enkhtur},
  
  booktitle={2022 IEEE 16th International Scientific Conference on Informatics (Informatics)}, 
  
  title={A 1D CNN-based model for IoT anomaly detection using INT data}, 
  
  year={2022},
  
  volume={},
  
  number={},
  
  pages={106-113},
  
  doi={10.1109/Informatics57926.2022.10083469}}



## License




