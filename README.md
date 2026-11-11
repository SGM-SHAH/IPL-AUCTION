# IPL Auction Simulator

Real-time multi-franchise Indian Premier League (IPL) auction simulation platform built with Firebase Realtime Database and vanilla JavaScript. Franchise owners can create private rooms, join with a room code, bid live on auction players, manage their team purse, and build their Playing 11 squad under official auction constraints.

## Overview

The application replicates the dynamics of an official IPL Mega Auction. A host creates an auction room and sets the bid timer. Franchise owners join using the generated room code, select their franchise, and participate in synchronized real-time bidding. When all players have been auctioned, owners configure their Playing 11 according to official overseas player limits and receive an algorithmic team rating score.

## Key Features

- **Synchronized Real-Time Bidding**: Powered by Firebase Realtime Database. All bid submissions, timer countdowns, and player rotations update instantaneously across all connected franchise clients.
- **Franchise Representation**: Supports all 10 IPL franchises (CSK, MI, RCB, KKR, SRH, RR, DC, PBKS, LSG, GT) with dedicated franchise color badges and owner identifiers.
- **Purse and Budget Enforcement**: Each team begins with a purse of 120.00 Crore. The system verifies available funds before validating a bid, preventing overspending.
- **Configurable Countdown Timers**: Hosts can configure bid countdown timers (5, 10, or 15 seconds). Every new highest bid resets the timer to ensure competitive bidding.
- **Automated Increments Ladder**: Bid increments automatically adjust based on player valuation thresholds (e.g. 20 Lakh to 50 Lakh steps).
- **Player Database**: Curated dataset of 150+ players categorized by role (Batsman, Bowler, All-Rounder, Wicketkeeper), skill rating, base price, overseas status, and career stats.
- **Franchise Trade Window**: Enables franchise owners to propose player-for-player trades during the auction with live accept/decline workflows.
- **Post-Auction Playing 11 Builder**: Dedicated interface to construct the final 11-player lineup from bought players.
- **Overseas Player Rule Enforcement**: Strictly restricts Playing 11 lineups to a maximum of 4 overseas players, matching official IPL guidelines.
- **Team Rating Algorithm**: Calculates an overall franchise rating (out of 10) based on player skill averages (80% weighting) and budget management efficiency (20% weighting).
- **Leaderboard and Rival Squad Tracker**: Live tracking of competitor purses, acquired rosters, and finalized Playing 11 lineups.

## Tech Stack

- **Client**: Vanilla JavaScript (ES6 Modules), HTML5, CSS3.
- **Realtime Database**: Firebase Realtime Database (SDK v9.23.0 modular).
- **Authentication**: Firebase Authentication (Anonymous sign-in for zero-friction room entry).
- **Hosting**: Firebase Hosting with rewrite rules for single-page application routing.
- **Typography**: Bebas Neue (display headers) and Poppins (interface elements).

## Project Structure

```
IPL-AUCTION/
├── public/
│   ├── favicon.svg          # Vector cricket ball favicon
│   └── index.html           # Main single-page application (UI, CSS, and Firebase logic)
├── database.rules.json      # Firebase Realtime Database security rules
├── firebase.json            # Firebase Hosting and database deployment configuration
├── .firebaserc              # Firebase project target mapping
├── players.json             # Source player registry with stats and base prices
├── .gitignore               # Environment and artifact exclusions
└── README.md                # Project documentation
```

## Database Schema

The Firebase Realtime Database is structured as follows:

```
rooms/
  └── {roomId}/
        ├── state: "waiting" | "auction" | "finished"
        ├── hostId: string
        ├── bidTimer: number (default: 15)
        ├── timer: number
        ├── currentPlayerIdx: number
        ├── currentBid: number
        ├── highestBidder: string (participant ID)
        ├── participants/
        │     └── {participantId}/
        │           ├── name: string
        │           ├── team: string ("CSK", "MI", etc.)
        │           ├── purse: number (default: 120000000)
        │           ├── squad: Array<Player>
        │           └── teamRating: string
        └── tradeRequest/
              ├── from: string
              ├── to: string
              ├── myPlayer: Player
              ├── targetPlayer: Player
              └── status: "pending" | "accepted" | "declined"
```

## Getting Started

### Prerequisites

- A modern web browser with JavaScript enabled.
- A Firebase project with Realtime Database and Anonymous Authentication enabled.
- A local static file server (e.g. Node `npx serve`, Python `http.server`, or Firebase CLI).

### Local Execution

1. Clone the repository:
   ```bash
   git clone https://github.com/SGM-SHAH/IPL-AUCTION.git
   cd IPL-AUCTION
   ```

2. Start a local server pointing to the `public/` directory:
   ```bash
   # Using Python 3
   python -m http.server 8000 --directory public

   # Or using Node.js
   npx serve public
   ```

3. Open your browser and navigate to `http://localhost:8000`.

### Firebase Configuration

If you are using your own Firebase project:
1. Open `public/index.html`.
2. Locate the `firebaseConfig` object inside the `<script type="module">` tag:
   ```javascript
   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_AUTH_DOMAIN",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_STORAGE_BUCKET",
     messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```
3. Update the fields with your Firebase project credentials.
4. Ensure **Anonymous Authentication** and **Realtime Database** are enabled in the Firebase Console.

### Deployment

Deploy to Firebase Hosting using the Firebase CLI:

```bash
# Login to Firebase
firebase login

# Deploy hosting and database rules
firebase deploy
```

## Auction Rules and Constraints

1. **Initial Purse**: Each franchise receives ₹120.00 Crore.
2. **Squad Minimum and Maximum**: Teams can build squads up to 25 players during the auction.
3. **Playing 11 Composition**:
   - Exactly 11 players must be selected.
   - Maximum of 4 overseas players permitted in the Playing 11.
4. **Rating Formula**:
   - `Skill Component = (Average Player Rating / 5.0) * 10` (80% weight)
   - `Budget Efficiency Component = (Remaining Purse / 120 Crore) * 10` (20% weight)
   - `Team Rating = (Skill Component * 0.8) + (Budget Efficiency Component * 0.2)`

## License

This project is licensed under the MIT License. See the LICENSE file for details.

