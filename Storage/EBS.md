# EBS - Elastic Block Store

## General

- Network drive that can be attached somewhere (tipically EC2 instance) - "Virtual Pen Drive". 
- Being a network drive, some latency should be expected.
- Allow data to be persisted even after instance is terminated.
- They are bound to an specific availability zone. Data can be moved to other AZs only via snapshots.
- Can be detached and attached to other instances quickly.
- Needs a provisioned size GBs and IOPS, but can be increased later.

## IMPORTANT

- DELETE ON TERMINATION: By default the ROOT volume is set to delete on termination but any additional volume by default is not set to delete. This behaviour can be changed using the Console when creating the EC2 instance. So, to preserve the root storage, you need to disable this option!
- An EBS may be attached to multiple instances and an instance may have multiple EBSs attached to it.