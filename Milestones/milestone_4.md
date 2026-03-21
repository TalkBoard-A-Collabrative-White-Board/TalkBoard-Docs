# **TalkBoard — Milestone 4 Documentation**

**Version**: v0.3.1 <br>
**Status**: Completed<br>
**Scope**: Frontend System (UI Architecture, Whiteboard UI, Real-Time Interaction, WebRTC Audio Integration)<br>

---

## **Objective of Milestone 4**

Milestone 4 focuses on building the frontend layer of TalkBoard and integrating it with the real-time Socket Service developed in Milestone 3. This milestone transforms TalkBoard from a backend-driven system into a fully interactive collaborative application.

The primary goals of this milestone are to:
- Establish a scalable and maintainable frontend architecture
- Enable users to create and join collaboration rooms
- Implement an interactive whiteboard UI
- Integrate real-time drawing and synchronization
- Provide visual presence using live cursors
- Ensure reliable board state restoration for all users
- Introduce WebRTC-based real-time audio communication
- Prepare the frontend for production deployment

This milestone ensures that users can visually and interactively collaborate in real time, completing the core experience of TalkBoard.

## **Milestone 4 Deliverables**
### **1. Frontend Foundation & Project Setup**

- Initialized frontend project
- Configured ESLint and Prettier for code consistency
- Designed base folder structure and scalable architecture
- Implemented routing system
- Created core page structure
- Established initial Socket.IO client connection

This sets up a clean and maintainable frontend environment ready for feature development.

### **2. Frontend Architecture & Socket Integration**

- Redesigned frontend folder structure for modularity
- Refactored socket connection setup and lifecycle management
- Centralized socket handling logic
- Improved separation of concerns across components and services

This ensures the frontend can scale alongside real-time features without becoming tightly coupled.

### **3. Room System (UI Layer)**

- Defined Room types
- Implemented Room component
- Built:
  - Room creation functionality
  - Room join functionality

This enables users to enter collaborative sessions through a structured UI flow.

### **4. Whiteboard UI & Drawing Controls**

- Implemented Canvas-based whiteboard UI
- Added freehand drawing tool (host-only)
- Introduced:
  - Stroke width controls
  - Color selection controls
  - Clear board functionality

This provides the primary interaction layer for collaborative drawing.

### **5. Real-Time Interaction & Presence**

- Defined cursor data structures
- Implemented remote cursor rendering for all participants
- Enabled real-time visual presence of users on the board

This improves collaboration awareness and mimics real-world interaction.

### **6. Board Synchronization & State Management**

- Implemented board synchronization for late joiners
- Enabled board state restoration for:
  - Mid-session joins
  - Rejoining users after disconnect
- Improved synchronization logic for consistency
- Fixed board visibility issues after reconnect

This guarantees that all users share a consistent and recoverable board state at all times.

### **7. WebRTC Audio Communication**

- Defined WebRTC types
- Built a WebRTC service layer
- Integrated audio communication into the frontend
- Enabled real-time voice communication between participants

This transforms TalkBoard into a communication + collaboration platform, aligning with its core vision.

### **8. Production Readiness & Stability**

- Updated SocketService integration for production environments
- Adjusted configurations for deployment compatibility
- Resolved Git merge conflicts and integration issues

This ensures the frontend is stable and ready for real-world usage.

## **Version Progression Summary**

- v0.2.0 – Frontend initialization, routing, and socket connection setup
- v0.2.1 – Room system (create/join), folder restructuring, socket refactor
- v0.2.2 – Whiteboard UI, drawing tools, and controls
- v0.2.3 – Cursor sharing, board sync, and state restoration
- v0.2.4 – Synchronization improvements and reconnect handling
- v0.3.0 – WebRTC audio communication integration
- v0.3.1 – Production configuration updates and fixes

## **Outcome of Milestone 4**

Milestone 4 successfully completes the user-facing layer of TalkBoard. At this stage, TalkBoard now has:

- A structured and scalable frontend architecture
- Room-based collaboration with UI support
- Interactive whiteboard with drawing controls
- Real-time synchronization across all users
- Live cursor tracking for presence awareness
- Reliable board state persistence during sessions
- Integrated WebRTC-based audio communication
- Production-ready frontend configuration

This milestone bridges the gap between backend capabilities and user experience, making TalkBoard fully usable as a collaborative tool.

## **Conclusion**

Milestone 4 delivers the complete interactive experience of TalkBoard, combining real-time systems, UI architecture, and communication features into a cohesive application.

With both backend (Milestone 3) and frontend (Milestone 4) systems in place, the project is now ready to advance into:

- Advanced collaboration features (permissions, access requests)
- Whiteboard enhancements (shapes, text, media)
- Performance optimization and scaling
- UI/UX refinement and polish

This milestone ensures that real-time features are scalable, reliable, and cleanly separated from the REST-based App Service.
