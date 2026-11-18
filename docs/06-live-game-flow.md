# 6. Live game flow

## Game lifecycle

```mermaid
stateDiagram-v2
    [*] --> Draft: Host creates quiz
    Draft --> Draft: Host edits questions
    Draft --> Running: Host presses "Start this quiz"
    Running --> Running: Players join and answer, host switches questions and reveals answers
    Running --> WinnersShown: Host presses "End Quiz and show winners"
    WinnersShown --> Draft: Host presses "Stop Quiz"
    Running --> Draft: Host leaves the page
    WinnersShown --> Draft: Host leaves the page
```

A quiz can be played again: after a game ends, the quiz returns to the draft state and can be started again. Results of the previous run are not kept.

## End-to-end scenario

```mermaid
sequenceDiagram
    actor Host
    participant FE as Frontend (SPA)
    participant BE as Backend
    actor Player

    Host->>FE: Start this quiz
    FE->>BE: START_GAME (quiz ID)
    BE-->>FE: GAME_DATA (questions, first question active, no players)
    Host->>Player: Shares game ID (outside the app)
    Player->>FE: Enters game ID, Join game
    FE->>BE: Is this game running? (REST)
    BE-->>FE: Yes
    FE->>BE: PLAYER_CONNECTED (user ID)
    BE-->>FE: GAME_DATA to everyone (player added)
    Player->>FE: Types answer, Send
    FE->>BE: ANSWER_QUESTION
    BE->>BE: Check against accepted answers
    BE-->>FE: GAME_DATA to everyone (player marked as answered)
    Host->>FE: Show answers
    FE->>BE: SHOW_ANSWERS
    BE-->>FE: SHOW_ANSWERS to everyone (answers with correct/wrong)
    Host->>FE: Next question
    FE->>BE: CHANGE_QUESTION
    BE-->>FE: GAME_DATA to everyone (new active question)
    Host->>FE: End Quiz and show winners
    FE->>BE: SHOW_WINNERS
    BE-->>FE: SHOW_WINNERS to everyone (players go to winners page)
    Host->>FE: Stop Quiz
    FE->>BE: END_GAME
    BE-->>FE: END_GAME to everyone
```

## Real-time messages

All live events go over one WebSocket connection per browser tab. Each message is JSON `{ "type": ..., "payload": ... }`.

### From clients to server

| Message | Sent by | Payload | Business meaning |
|---|---|---|---|
| `START_GAME` | Host | quiz ID | Start the quiz live |
| `PLAYER_CONNECTED` | Player | user ID | Join the running game |
| `CHANGE_QUESTION` | Host | question ID | Make a question active |
| `ANSWER_QUESTION` | Player | question ID, answer text, user ID | Send an answer |
| `SHOW_ANSWERS` | Host | question ID | Reveal answers to the question |
| `SHOW_WINNERS` | Host | none | Show the winners table |
| `END_GAME` | Host | none | Stop the game |

### From server to clients

| Message | Payload | Business meaning |
|---|---|---|
| `GAME_DATA` | full live game state | Players, questions or active question changed |
| `SHOW_ANSWERS` | each player's answer to the question with correct/wrong | Reveal |
| `SHOW_WINNERS` | none | Go to the winners table |
| `END_GAME` | none | The game is over |
| `FAULT` | error text | The action was rejected (game already running, no game running) |

Server messages are sent to **every** connected client, not only to the players of a specific game. This works only because one game runs at a time (BR-20).

The server does not check who sends a message: any connected client can send host messages such as `CHANGE_QUESTION` or `END_GAME` (see [GAP-3](08-gaps-and-backlog.md)).
