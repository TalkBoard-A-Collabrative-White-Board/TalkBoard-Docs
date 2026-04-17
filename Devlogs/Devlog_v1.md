# DevLog — TalkBoard v1

**Status:** Shipped ✓<br>
**Stack:** React · Node.js · Express · Supabase (PostgreSQL) · Prisma · Socket.IO · MongoDB · WebRTC<br>
**Repos:** TalkBoard-Frontend · TalkBoard-AppService · TalkBoard-SocketService · TalkBoard-Docs<br>

---

## Overview

TalkBoard is a real-time collaborative whiteboard with integrated audio. The host controls the session — guests join via a time-limited URL, draw together on a shared canvas, and communicate over WebRTC audio without needing a separate call running in the background.

The goal was never to clone Miro or Figma. It was to build something focused: a session has a clear start, a clear end, and one person in control of it.

---

## Architecture Decision — Why Two Backend Services?

From the start I split the backend into two separate services: **AppService** and **SocketService**. The reason was straightforward — they do fundamentally different things and mixing them would've made both harder to reason about.

**AppService** handles user data, auth, and persistent state. It uses **Supabase** (managed PostgreSQL) with **Prisma** as the ORM — the same role Mongoose plays in a MongoDB setup, just for a relational database. Supabase made sense here because it comes with auth built in, which saved a lot of setup time on the user management side.

**SocketService** handles everything real-time: room state, drawing events, cursor sharing, WebRTC signaling. It uses **MongoDB** for board persistence — board state needed to survive across socket reconnections and late joiners, and MongoDB fit that use case well.

Two services, two clearly scoped responsibilities.

---

## How the Build Progressed

### v0.0.1 — v0.0.7 · AppService Foundation

The first phase was entirely infrastructure. GitHub organization set up with separate repos for frontend, AppService, SocketService, and docs. Kanban board created for tracking.

AppService came together with TypeScript, ESLint, Prettier, Prisma schema, Supabase integration, user routes and controllers, and a global error handling layer. Added Zod for runtime schema validation on incoming requests — catching malformed data at the boundary before it touches the database.

By v0.0.7 the AppService had a clean, consistent error response flow across all endpoints.

### v0.1.0 — v0.1.6 · SocketService

This was the most complex phase of the build.

Started with basic Express + Socket.IO setup, then built out the room and user store layers to track connected users and intermediate room state in memory.

**The hardest bug of the entire project happened here.**

When users connected, a duplicate socket server was being created. The root cause: I had initialized a Socket.IO server instance in `index.js` and separately in `socket/index.js`. Both were running. The fix was simple once I found it — stop calling the initialization twice and route everything through a single instance — but finding it took a while because the symptoms were subtle at first.

Drawing came next. Freehand drawing needed throttling and batching to avoid flooding the socket with events on every mouse move. There was also a recursion bug in `drawing.handler` that surfaced during this work — the exact cause is fuzzy now, but it was caught and fixed before it caused bigger issues.

v0.1.4 added shapes, text input, erase, and clear board. v0.1.5 added cursor sharing (broadcasting each user's cursor position in real time), MongoDB integration, and automatic board state restoration — so if you disconnect and rejoin, or join mid-session, you see the current board rather than a blank canvas.

v0.1.6 added the WebRTC signaling layer in SocketService, setting up the event types and handlers that the frontend would later use to establish peer connections.

### v0.2.0 — v0.2.4 · Frontend

Frontend started with routing, base folder structure, and Socket.IO client setup. v0.2.1 involved a full folder structure redesign early on — the initial layout wasn't scaling well, so it got refactored before too much was built on top of it.

Canvas implementation landed in v0.2.2: freehand drawing (host-only), stroke width controls, color picker, clear board. v0.2.3 and v0.2.4 focused heavily on synchronization — remote cursor rendering, board state for late joiners, and making sure rejoining users always see the correct current board state.

### v0.3.0 — v0.3.1 · WebRTC Audio + Ship

WebRTC audio went from nothing to working in a single focused session. The signaling infrastructure was already in SocketService from v0.1.6, so this was about building the service layer and peer connection management on the frontend side. Once the signaling flow was clear, it came together in one shot.

v0.3.1 was production config cleanup, deployment adjustments, and resolving merge conflicts before the final ship.

---

## Challenges

**Duplicate socket server creation** — the biggest friction point. Two initialization calls producing two running instances. Subtle to diagnose, simple to fix.

**Board sync for late joiners** — making sure someone joining mid-session or reconnecting after a drop always gets the current board state required careful coordination between MongoDB persistence and the in-memory board store.

**Freehand drawing performance** — raw mouse events over a socket would've been unusable. Throttling and batching the freehand buffer were necessary to keep drawing smooth, and getting that right took a few iterations.

**WebRTC signaling** — the signaling protocol for establishing peer connections is low-level and unforgiving. The SocketService layer handling offer/answer/ICE candidate exchange had to be solid before the audio would work reliably.

---

## What's Next

**v2** — deeper collaboration features, improved canvas tooling, more refined permission controls.

**v3** — persistent boards, session history, and likely AI-assisted features on top of the whiteboard layer.

v1 does exactly what it set out to do. Real-time collaborative whiteboard, host-controlled sessions, time-limited URLs, and live audio — all in one place.

---

*— Arjun O*
