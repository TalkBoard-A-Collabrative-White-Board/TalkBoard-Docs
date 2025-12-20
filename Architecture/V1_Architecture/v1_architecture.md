# TalkBoard - v1 Architecture Documentaion

## **1.Overview**
TalkBoard is a real-time collabrative whiteboard application designed for low latency communication and controlled collaboration.

The v1 architecture focuses on **correctness, clarity and a strong foundation** , while intentionally avoiding premature optimization or 
overengineering.

The system follows a **service-oriented architecture**, with strict seperation between authentication and real-time communication.

## **2.Architectural Goals (v1)**
The primary goals of talkBoard v1 are:
- Low-Latency real-time collaboration
- Secure and isolated authentication flow
- Simple and maintainable implementation
- Future scalabiltity without architectural rewrite

## **3.Architecture Diagram**
TalkBoard v1 is represented using two **complementary diagram**.Both diagram describes the **same v1 system**, viewed from 
different abstraction levels.

### **3.1 Diagram 1 - Logical Architecture(Conceptal View)**
**Purpose**:
- Explain system responsibilities
- Show component boundaries
- Describe communication relationships

**Key Characteristics:**
- Client communication independently with:
  - Auth Server
  - Socket Server
- Each services owns its own data
- No direct dependency between Auth Server and Socket

### **3.2 Diagram 2 - Infrastructure Architecture(Deployment View)**
**Purpose**:
- Show how the system is deployed
- Represent scalability intentions
- Reflect real-world server topology

**Key Characteristics:**
- Socket Server shows as horizontally scalable
- Independent Auth Server
- Dedicated databases per service
- Clear client-to-service communication paths

## **4.High-Level Architecture**
```
client
 |__ Auth Server __ Auth Database
 |__ Socket Server __ Socket Database
```
Each service is indepndently deployable and scalable.















