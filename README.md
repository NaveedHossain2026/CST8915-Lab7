# CST8915 Lab 7

**Student Name**: Naveed Hossain
**Student ID**: 0410818822 
**Course**: CST8915 Full-stack Cloud-native Development
**Semester**: Winter 2026

---

## Demo Video

🎥 [Watch Demo Video](https://youtu.be/tDTJIIkydiY)

---

## Written analysis of the RabbitMQ configuration issues

### 1. Is RabbitMQ Stateless or Stateful?

RabbitMQ is a Stateful application. This means that, unlike a stateless web server (like the store front), which simply processes requests, RabbitMQ has the task of holding data (messages, orders, and queues). In order for RabbitMQ to function correctly, it has to remember the state of the messages it handles, even if the process is stopped.

### 2. The implications of running RabbitMQ without persistent storage

Running RabbitMQ without persistent storage in Kubernetes effectively treats it as a stateless service, posing critical risks to data integrity. Kubernetes pods use temporary storage by default, and any messages, orders, and queue configurations are lost if the pod crashes and restarts. A new pod will have no data when it starts, leading to the permanent loss of messages that are not yet processed.


### 3. What happens when the RabbitMQ pod is deleted or restarted

If the RabbitMQ pod is deleted or restarted, all the data is lost. This is because it is running as a regular deployment without persistent storage, so the Kubernetes system will assume it is a temporary pod and delete all data inside the pod as soon as the pod stops. If a new pod is created, it will have no data at all, and all pending messages (like orders waiting to be processed) will be permanently lost, as will other services that might be affected by the missing expected queues.

### 4. Potential solutions to this problem

To fix the data loss issue in RabbitMQ, it needs to be moved from temporary storage to persistent storage. RabbitMQ should run on a StatefulSet. A StatefulSet will give the pod a permanent identity and will ensure the pod reconnects to the storage even if it is restarted. Persistent Volume Claims (PVCs) should also be used to attach a real storage disk, like an Azure Disk. This stores the messages, queues, and configurations outside the container, so that in the event of a pod crashing or being replaced, the data is safe and can be restored automatically.


### 5. Does using Azure Service Bus solve the issues identified with RabbitMQ Configuration in this Lab?

Yes, using Azure Service Bus does solve the issues identified with the RabbitMQ configuration in this lab. The main problem was data loss due to no persistent storage. Azure Service Bus fixes this issue by safely storing messages, so they aren’t lost if something crashes or restarts. It also removes the need to manage Kubernetes components like pods, storage, and configurations, reducing the risk of misconfiguration. In addition, Azure handles scaling, availability, and reliability, which reduces the risk of downtime or message loss. However, the downside is less control and possibly higher cost than managing RabbitMQ directly, but overall it’s a simpler and more reliable solution, especially for production environments. 


---

## Acknowledgments

I used Gemini to troubleshoot errors during the lab and to help with background research for the written analysis.
