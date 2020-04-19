# Capstone: Automating Fall Detection with Machine Learning

<span style="display:block" align='center'>![](https://lh3.googleusercontent.com/N7P7ev9yDfH3a2jymfuFhypfQFSY6erYoPiKmx6cZjOGZE6UjywWKlITvvx1ErRSdPuAHAcOBWCRXf31JgsjsQZNXn5g6Lxr-TMJCCXcLUBkosMuQfGm8RDDFq7O2uuVtWTGJwBNgw4)![](https://lh3.googleusercontent.com/3FMh2SiGmx5xJMLhDwroL9wlX2Vb4Pwq5W64AjHCH-13APt1od9o1d32juXAn-4PHl6IFEr-ohc07q-LiuhU23EzK1bTScnQ63_I34RpcLBjsTnDHG07XsEP-bfCDTk2aKN7sjbNSxE)![Processing video to detect a fall on camera](https://lh3.googleusercontent.com/604dTSvSMBWxoDQ7W_xM2UNIsWtywuq8WOPyn9qkdb9bKQ7OAbpwuF-1Ns_zftkrWRi7Kgm88R4P2Hp0tCBRAAmxbAMRbOoyUZ1HjyOQIh1iEAoqMISs0-bZHKh_sbgoPrqpa-jX_FI)
*Processing video to detect a fall on camera*


## Context 
In my final two semesters of college I completed a capstone project in coordination with a local company - Vivint SmartHome (VSH). VSH has a line of in-home cameras that customers purchase for home security. Two fellow classmates and I were tasked with prototyping a new feature for them: automatically detecting if a person has fallen over. Our little 3-person team was multidisciplinary, and, as is the case in most undergraduate group projects, we each had varying interests in different aspects of the project. I was very interested in ML at the time and relished the opportunity to tackle a real-world problem with ML. As such, I led the effort on researching and designing the data processing, modeling, and overall ML approach, and that will be the focus of this blog post. 

If you're interested in using ML to process video, this walkthrough could prove highly useful; it's a simple technical design that's great for beginners! What follows is a conceptual overview of my specific problem, a description of my technical approach, and a presentation of my results.



## Background
Falling is a significant problem for elderly people; over 30% of population age 65+ falls annually, and falls are responsible for over 50% of hospitalizations in that demographic. Injuries and hospitalization as the result of a fall are a large risk. VHS has existing in-home security cameras that are connected to the internet. With this project, I developed an algorithm and data processing pipeline to automatically detect a fall. If this were put into production it could notify family members or emergency services immediately in the event of a fall, if the customer enabled this functionality. (For example, if a family installs such a system in the home of their elderly relative, they could enable this feature. When grandkids come to visit, they could disable it to avoid unnecessary alerts.)

### Initial Assumptions
Since this was a simple proof-of-concept project for VHS, we allowed ourselves several limiting assumptions in our data set. These assumptions were:
-   only one person in the frame at a time
    
-   the falling motion of the individual is perpendicular to the camera
    
-   no concept of recovering from a fall, i.e. if a fall is detected on camera, it is classified as a fall even if the individual gets back up after the fall.
    
-   fall event not occluded; the majority of the fall event is visible to the camera and not hidden behind furniture, etc.

## Data Collection
Since no relatively simple, labeled data set was available to us, we curated our data set ourselves --- that's right, we filmed ourselves falling over and over again in a controlled environment, much to the pleasure of our fellow classmates and professors. 

**![](https://lh3.googleusercontent.com/yLIW123xhS-M4f8RM4OZdpfsA4Q_2_8iXUpd0X4MbEPy9V7QmSH9Z6WR94hdI2Th3HZ_G-HC-KnBB2M_XTdCtvflZUrGC_dsKJPbZv-zqrWGl8IYNqoTDv29zX5XefiClyOAb0uoEtg)**

Subjects purposefully varied in height, width, and color of apparel to give variety to our data set and prevent models from learning from inappropriate, non-generalizable features.

A data augmentation process was employed to generate multiple two-second clips (60 frames for 30fps cameras) from a single video. Borrowing the concept of stride from CNNs, a stride length of 30 frames was used to capture overlap between clips. The process is shown below with an eight second video (240 frames), which generates seven distinct two-second data points. 

**![](https://lh4.googleusercontent.com/Bp-Bnv7Y6ePZf8bKVqu2ch015maoqqeZPPZtcpRWvOYA0-frmv4ouvGMD4hY5KV8P2VRR-BZ-L-Wuh4bdVcozF-skSIjW0LQEC3NyKvBr4gU6QOzzkVRdXZs3kna6yXyCZVjIJeIJ4E)**

This resulted in a dataset of 424 2-second videos. To match the natural imbalance between fall and non-fall videos, 20% of our dataset are clips of fall events and the other 80% are clips of non-fall events.
