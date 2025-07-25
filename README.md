# 3D-Bridge-App
single and multi social app


![image](./3D-Bridge-Game-(Offline).png)


in progress ... this is the boiler/lunch room ... check out the menu TBA
7/25.25

Phase 1: Core Multiplayer & User Foundation (Firebase)

This is the most critical phase. We need to reintroduce Firebase and build the backbone for all user interaction and gameplay.

1. User Authentication & Profiles:

    Task: Implement a persistent user login system (Email/Password or Google/Social Sign-in).

    Firebase Structure:

        Create a users collection in Firestore.

        Each user document will have a uid (from Firebase Auth) and store data like:

            displayName (customizable username)

            email

            profileImageUrl (for avatars)

            friends (an array of other user uids)

            friendRequests (incoming/outgoing requests)

            stats (wins, losses, etc.)

2. Game Lobby & Matchmaking:

    Task: Create a system for players to host, find, and join games.

    Firebase Structure:

        Create a games collection in Firestore.

        Each game document will have a unique ID and store:

            hostId: The uid of the player who created the game.

            players: An array of objects, each containing a player's uid, displayName, and their position (north, south, etc.).

            gameState: (e.g., 'waiting', 'bidding', 'playing', 'finished').

            gameData: All the gameplay variables we currently have locally (hands, trick, contract, etc.).

            isPublic: A boolean to allow for public or private (invite-only) games.

3. Real-time Game Sync:

    Task: Convert all local game logic to use Firebase.

    Implementation:

        Remove the local gameData object.

        Use Firebase's onSnapshot listener to watch the active game document for changes.

        When a player makes a bid or plays a card, the action is written to the game document in Firestore.

        onSnapshot will then automatically push that update to all other players' clients, keeping everyone's game perfectly in sync.

Phase 2: Building the Social Experience

With the multiplayer foundation in place, we can add the social layers.

1. In-Game Text Chat:

    Task: Implement a real-time chat box within the game screen.

    Firebase Structure:

        Inside each game document in the games collection, create a sub-collection called chat.

        Each document in chat will be a single message, containing:

            senderId: The uid of the player.

            senderName: The player's display name.

            message: The text of the message.

            timestamp: A server timestamp for ordering.

2. Friends & Presence System:

    Task: Allow users to add friends and see who is online.

    Implementation:

        Friend Requests: Create UI for sending, accepting, and declining friend requests. This logic will update the friends and friendRequests arrays in the user documents.

        Online Status (Presence): Use Firebase's Realtime Database (which is excellent for this) or a simple Firestore "status" collection. When a user opens the app, they set their status to 'online'. When they close it, they are set to 'offline'. This allows you to build a "Friends Online" list.

Phase 3: Advanced Communication (The "Social Media" Feel)

This is where it becomes a true social platform. Integrating voice and video is complex and requires a service like WebRTC.

1. Voice & Video Chat Integration (WebRTC):

    Task: Allow players in a game to talk to and see each other.

    High-Level Plan:

        Signaling: We will use Firebase as a "signaling server." This is not for sending the video/audio itself, but for players to securely exchange the technical information (IP addresses, codecs) needed to connect directly.

        Peer-to-Peer Connection: Once the signaling is complete, each player's browser will establish a direct WebRTC connection to the other players in the game. The video and audio streams will flow directly between them, not through our server.

        UI Controls: We will need to add UI elements on the game screen for muting the mic, disabling the camera, and controlling volume for each player.

2. Admin/Host Controls:

    Task: Give the game host administrative powers.

    Features:

        Ability to kick a player from the game lobby.

        Power to mute a specific player's audio/video for everyone else in the game.

        Option to make a public game private (or vice-versa) before it starts.

This roadmap gives us a clear path from where we are now to a full-featured social Bridge application. We can tackle it one phase at a time, starting with the critical multiplayer foundation.

[current index.html v.1.0](https://pdragonlabs.github.io/3D-Bridge-App/)
