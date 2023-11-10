---
layout: default
title: EBS x EFS Comparison
parent: Storage
grand_parent: AWS Services
---

# EBS x EFS Comparison
{: .no_toc }

---

|                              | EBS                                                                                                               | EFS                                                    | Instance store                          |
|------------------------------|-------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------|-----------------------------------------|
| **Attachment**               | Up to 1 EC2 Instance in general in the same AZ                                                                    | Multiple EC2 Instances simultaneously in different AZs | Physically attached to the EC2 instance |
| **Availability Zones**       | Single AZ                                                                                                         | multi-AZ                                               | Same as EC2 instance                    |
| **Data Migration**           | Requires a snapshot to be taken and restored. Backupt uses IO, so it shouldn't be executed when there is workload | Not applicable as it can be attached to different AZs  | -                                       |
| **EC2 Instance Termination** | Root Volumes are terminated by default                                                                            | Not impacted                                           | Terminated with the instance            |
| **OS Compatibility**         | Linux and Windows                                                                                                 | Only Linux                                             | Linux and Windows                       |
| **Cost**                     | Lower                                                                                                             | Higher                                                 | EC2 Cost                                |
| **Lambda integration**       | No                                                                                                                | Yes                                                 | Lambda has up to 10MB of ephemeral storage                             |

