# Bitasmbl-Lightweight-Chat-App-49cafe-Nodar_Mebunia

## Description
Build a web application that allows users to join anonymous chatrooms and exchange messages in real-time using WebSockets. The focus is on fast communication, simple interface, and responsive updates without requiring user registration.

## Tech Stack
- Objective-C
- FastAPI
- Socket.IO

## Requirements
- Allow users to join chatrooms without authentication
- Gracefully handle disconnected users and reconnections
- Handle multiple chatrooms simultaneously
- Display messages with timestamps and user identifiers (anonymous)
- Send and receive messages in real-time using WebSockets

## Installation
1. Clone the repository (replace the repo name if different):
   - HTTPS: git clone https://github.com/MrBitasmblTester/Bitasmbl-Lightweight-Chat-App-49cafe-Nodar_Mebunia.git
   - SSH: git clone git@github.com:MrBitasmblTester/Bitasmbl-Lightweight-Chat-App-49cafe-Nodar_Mebunia.git

2. Backend (FastAPI + Socket.IO):
   - Create and activate a Python virtual environment:
     - python3 -m venv venv
     - source venv/bin/activate
   - Install required Python packages:
     - pip install fastapi uvicorn python-socketio
   - (Optional) If you use async tooling, ensure your environment has an appropriate ASGI server (uvicorn is installed above).

3. Objective-C client setup (iOS/macOS client using Objective-C and Socket.IO client library):
   - Ensure Xcode is installed.
   - If using CocoaPods for dependency management (recommended):
     - Install CocoaPods (if not already installed): sudo gem install cocoapods
     - In the client directory, create or update Podfile to include a Socket.IO client library appropriate for Objective-C/Apple platforms.
     - Run pod install
   - Open the Xcode workspace/project produced by CocoaPods to build the Objective-C client.

Notes:
- This repository is organized for a FastAPI backend and an Objective-C client. Follow the project layout in the repo for exact paths to backend and client code.

## Usage
Backend (FastAPI + Socket.IO):
1. Activate the Python virtual environment:
   - source venv/bin/activate
2. Start the server (example using uvicorn):
   - uvicorn main:app --host 0.0.0.0 --port 8000 --reload
   - Adjust the module:path (main:app) to match the backend entrypoint in the repository.

Objective-C client (iOS/macOS):
1. Open the client Xcode workspace/project produced after CocoaPods setup.
2. Build and run the app in Simulator or on a device.
3. Use the client UI to join anonymous chatrooms (room identifiers) and send/receive messages in real-time via Socket.IO.

Behavior expectations:
- Clients connect to the backend via Socket.IO WebSocket transport.
- Users join chatrooms by sending a room identifier; no authentication is required.
- Disconnections and reconnections should be handled by the client and server Socket.IO logic so users rejoin rooms on reconnect.
- Messages displayed include a timestamp and an anonymous user identifier emitted by the client or assigned by the server.

## Implementation Steps
1. Initialize FastAPI project structure and create an ASGI app (e.g., app = FastAPI()).
2. Add python-socketio server integration with FastAPI (attach a Socket.IO server to the FastAPI ASGI app or run alongside using ASGI middleware).
3. Implement in-memory room management structures (e.g., dict mapping room IDs to connected session IDs). Avoid persistent storage since requirements do not specify it.
4. Implement Socket.IO events:
   - connect: accept incoming connections;
   - join: allow a connection to join a named room (no authentication required);
   - message: broadcast messages to the specified room including server-side timestamp and an anonymous user identifier;
   - disconnect: remove connection from room mappings and broadcast presence updates as needed.
5. Ensure reconnection logic: rely on Socket.IO client/server reconnection features and, on server reconnect, re-associate the client session with previously joined rooms if the client requests rejoin after reconnection.
6. Include message payload structure that contains: { room, user_id (anonymous), message, timestamp } and ensure timestamp is set server-side at receipt.
7. Implement support for multiple chatrooms by routing join and message events using the provided room identifier and using Socket.IO rooms feature.
8. Implement minimal HTTP endpoints if needed for health checks or serving a static client bundle (optional and only if present in the repo).
9. Build the Objective-C client UI to: connect to the Socket.IO backend, send join events with a room identifier, display messages with timestamps and anonymous identifiers, and handle reconnects by listening to Socket.IO connection events and rejoining rooms.
10. Test end-to-end: run the FastAPI server, launch multiple Objective-C client instances, join the same and different rooms, verify real-time message delivery, timestamp display, anonymous identifiers, and graceful handling of disconnects/reconnects.

## API Endpoints (Optional)
- Endpoint: /socket.io/
  - Method: WebSocket (Socket.IO)
  - Description: Socket.IO endpoint used for real-time connections. Supports events such as connect, join (room), message (send/receive), and disconnect. The server uses this endpoint to handle room joins, broadcasting, and reconnection behavior.