### Docker Storage 
- Storage Drivers: Handles all of volume mount behaviors 
- Volume Drivers: 
  - File system: '/var/lib/docker/', 
  - Layered architecture: 
    - layer1: base ubuntu layer
    - layer2: changes in apt packages
    - layer3: changes in pip packages
    - layer4: source code
    - layer5: update untrypoint 
    Each layer has independent storage between them, and if they already used previous layer, they don't recreate previous layer which it used. 
    Those layer (1-5) considers as 'Image Layers'
    - Container Layer: layer6; container layer (R/W), it can't edit anything in 'Image Layers' 
    - Image Layer: Copy-On-Write which means image layer file is modified in 'Container Layer' 
  - Volumes: 'docker volume create data_volume'
             'docker run -v data_volume:/var/lib/mysql mysql'  <- mount volume at mysql inside Docker host 
             Volume Mount & Binded Mount
    - Volume Mount: it mounts from volume directory
    - Bind Mount: it mounts from any location on Docker host 
   - Storage Driver: handled all of this behavior (ex. Overlay, Overlay2, ZFS, Device Mapper, ...)

---
### Docker Volumes Plugin 
- Local, Azure File Storage, Convoy, NetApp, RexRay (AWS EBS), VMware vSphere Storage ... 

---
### Container Storage Interface
After shown different container runtime source such as docker, rkt, cri-o, their sources need to handle and so **Container Runtime Interface** shown. It is orchestrated and handled. 
  - CRI: handled rkt, docker, cri-o
  - CNI: weaveworks, flannel, cilium
  - CSI: portworx, Amazon EBS, dell EMC, GlusterFS <- 3rd vendor
         MESOS <- from Kubernetes 
---
### Volume
- Docker Container: they only use volume when it request and used. 
  Data process -> finish -> deleted in Volume [Example]()
- Communicates with External Storage? We can use CSI by 3rd party 

---
### Persistent Volume
- User deploy the pod what they need to configure. Good? 
- Uses Centralize Storage: [Persistent Volume]() 

---
### Persistent Volume Claims 
- Administrator Claim & User Claim
- All claim bound in single PV by Selector and Label.
- One to one relationship, if PVC is not availble? -> Pending 
- Delete? What happend after it? Retain / Delete / Recycle(Scrubbed)

