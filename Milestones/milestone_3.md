# **TalkBoard — Milestone 3 Documentation**

**Version**: v0.1.6 <br>
**Status**: Completed<br>
**Scope**: Socket Service (Real-Time Collaboration, Whiteboard Sync, Presence & Signaling)<br>

---

## **Objective of Milestone 3**

Milestone 3 focuses on designing and implementing TalkBoard’s Socket Service, the real-time backbone of the application.
While Milestone 2 established a stable REST-based App Service foundation, this milestone introduces event-driven, low-latency communication required for collaborative whiteboard sessions.

The primary goals of this milestone are to:
- Enable real-time whiteboard synchronization
- Manage collaborative rooms and user presence
- Enforce role-based permissions (host vs participant)
- Persist and restore board state reliably
- Lay the groundwork for WebRTC-based audio communication

This milestone ensures that TalkBoard can support multiple concurrent users interacting in real time with a consistent and recoverable shared state.

## **Milestone 3 Deliverables**
### **1. Socket Service Foundation**

- Initialized SocketService as an independent service
- Configured ESLint and Prettier for consistent code quality
- Set up a basic Express server
- Installed and configured Socket.IO
- Verified stable socket server startup and connectivity
- This establishes the real-time infrastructure required for collaborative features.

### **2. Socket Core Architecture & Room System**

- Introduced modular socket initialization (index.ts)
- Implemented socket server manager to support multiple instances
- Centralized socket event registration
- Implemented room handling logic (room.handler.ts)
- Added in-memory room state management (room.store.ts)
- Fixed duplicate socket server creation during reconnections

This enables session-based collaboration where each TalkBoard session maps to a dedicated socket room.

### **3. User Presence & Lifecycle Management**

- Implemented user state tracking (user.store.ts)
- Added handlers for:
  - User leave events
  - Unexpected socket disconnects
- Ensured proper room cleanup and state consistency
- Implemented automatic host transfer when the current host leaves or disconnects

This guarantees stable room ownership and accurate user presence across sessions.

### **4. Whiteboard Drawing System**

- Defined drawing data types and interfaces
- Introduced a dedicated drawing service logic layer
- Implemented drawing event handlers
- Registered drawing module within the socket lifecycle
- Added throttling and batching support for freehand drawing
- Fixed buffer recreation and recursion issues in drawing handlers

The drawing system is optimized for performance and supports smooth real-time collaboration.

### **5. Permission & Role Enforcement**

- Implemented a centralized permission validation utility
- Enforced host-only access for restricted actions
- Defaulted participants to read-only mode
- Enabled controlled drawing access through host authority

This ensures predictable and secure collaboration behavior across all sessions.

### **6. Advanced Whiteboard Features**

- Implemented board erase and clear functionality
- Added shape drawing support (rectangle, circle)
- Added text input support on the whiteboard
- Introduced in-memory board state storage (board.store.ts)
- Maintained board state consistency across all connected users

This completes the core feature set expected from a collaborative whiteboard.

### **7. Board Persistence & Recovery**

- Integrated MongoDB with SocketService
- Implemented board state persistence in the database
- Enabled automatic board restoration for late joiners or reconnecting users
- Ensured board data survives socket server restarts while active

This provides fault tolerance and continuity during live sessions.

### **8. Cursor Sharing & Presence Awareness**

- Implemented cursor sharing handlers and payload definitions
- Enabled real-time cursor position broadcasting
- Ensured low-latency, ephemeral cursor updates without persistence
- This improves collaboration awareness and user experience.

## **9. WebRTC Signaling Foundation**

- Defined WebRTC event types
- Implemented socket-based WebRTC signaling handlers

This lays the groundwork for audio communication in future milestones.

## **Version Progression Summary**

- v0.1.0 – SocketService initialization and server setup
- v0.1.1 – Socket core architecture and room management
- v0.1.2 – User lifecycle and disconnect handling
- v0.1.3 – Drawing system, permissions, and host transfer
- v0.1.4 – Shapes, text, board clearing, and in-memory persistence
- v0.1.5 – Cursor sharing and MongoDB board persistence
- v0.1.6 – WebRTC signaling event foundation

## **Outcome of Milestone 3**

Milestone 3 successfully transforms TalkBoard into a real-time collaborative system. At this stage, TalkBoard now has:

- A production-ready Socket Service
- Stable room-based collaboration
- Real-time whiteboard synchronization
- Role-based permission enforcement
- User presence and host continuity
- Persistent and recoverable board state
- Cursor awareness for improved collaboration
- A signaling layer ready for WebRTC audio integration

This milestone ensures that real-time features are scalable, reliable, and cleanly separated from the REST-based App Service.

## **Conclusion**

Milestone 3 establishes the real-time core of TalkBoard.
With both App Service and Socket Service foundations in place, the project is now ready to focus on frontend integration, WebRTC audio communication, and user experience refinement.
