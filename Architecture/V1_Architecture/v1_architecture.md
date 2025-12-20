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

## **5.Core Component**

### **5.1 Client(Web Application)**
The client is a browser-based application built using React and TypeScript.
**Responsibilities:
- Render whiteboard UI and tools
- Handle user interaction
- Perform authentication flow
- Establish and maintain WebSocket connections
- Send and receive real-time events
**Characteristics:**
- Stateless
- No direct database access
- Uses JWT for authentication
- Connects separately to Auth Server and Socket Server

### **5.2 Auth Server**
The Auth Server manages authentication and authorization.
**Responsibilities:**
- OAuth login (Google / Microsoft)
- User identity verification
- JWT generation and signing
- Basic role assignment (host / participant)
**Characteristics:**
- Stateless
- REST-based
- Not involved in real-time message flow
- Scales independently

### **5.3 Auth Database**
The Auth Database stores user-related data.
**Stored Data:**
- User ID
- Name
- Email
- Profile image
- Authentication provider metadata
**Design Rationale:**
- Isolates sensitive data
- Improves security and maintainability
- Avoids coupling auth logic with real-time systems

**5.4 Socket Server**
The Socket Server handles all real-time interactions.
**Responsibilities:**
- WebSocket connection management
- Room creation and joining
- Real-time whiteboard synchronization
- Event broadcasting
- Permission enforcement (host vs participant)
- Audio signaling (future WebRTC support)
**Characteristics:**
- Optimized for low latency
- JWT verified locally (no blocking auth calls)
- Minimal persistent storage dependency
- Horizontally scalable by design

### **5.5 Socket Database**
The Socket Database stores session-related data.
**Stored Data (v1 / optional):**
- Session metadata
- Room identifiers
- Temporary session state
**Design Rationale:**
- Separates ephemeral session data from auth data
- Allows real-time logic to remain fast
- Persistence can be optional in v1

## **6. Authentication Flow**
1. Client initiates login request
2. Request is sent to Auth Server
3. Auth Server validates credentials via Auth Database
4. Auth Server issues a signed JWT
5. Client stores JWT securely
6. JWT is attached to subsequent WebSocket connections
Authentication is completed before any real-time interaction begins.

## **7. Real-Time Communication Flow**
Client establishes WebSocket connection with Socket Server
1. JWT is sent during connection handshake
2. Socket Server verifies token signature
3. Client joins a session (room)
4. Real-time events are exchanged via WebSocket
5. Optional persistence is handled via Socket Database
The Auth Server is not involved in real-time message exchange.

## **8. Authorization & Permissions (v1)**
Authorization logic is enforced within the Socket Server.
**Examples:**
- Only hosts can:
  - Clear the board
  - Mute participants
  - Remove users
- Participants have restricted write access
- Permission checks occur at event-handling level
This avoids unnecessary network calls and reduces latency.

## **9. Design Principles**
The architecture follows these principles:
- **Separation of Concerns** <br>
Authentication and real-time logic are isolated.
- **Low Latency First** <br>
Real-time services avoid blocking dependencies.
- **Scalability by Design** <br>
Services can scale independently.
- **Security-Oriented** <br>
Auth data and session data are isolated.
- **Incremental Evolution** <br>
Infrastructure enhancements can be added later without redesign.

## **10. Conclusion**
TalkBoard v1 architecture provides a clean, maintainable, and scalable foundation for a real-time collaborative application.
It prioritizes simplicity and correctness while remaining extensible for future growth.




