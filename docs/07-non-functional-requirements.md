# 7. Non-functional requirements

Status values are explained in the [README](../README.md#status-legend).

## Security

| ID | Requirement | Status |
|---|---|---|
| NFR-S1 | Passwords shall be stored hashed (for example bcrypt or argon2), never in plain text. | Not implemented: passwords are stored and compared as plain text |
| NFR-S2 | The server shall check the user's session on every protected request; the session token shall not be guessable and shall not be readable by page scripts (`HttpOnly`, signed or random token). | Not implemented: the session cookie contains the plain user ID; the frontend only checks that the cookie exists |
| NFR-S3 | Only the host of a quiz shall be able to change it, delete it or run it. | Not implemented: question, answer, quiz and user endpoints do not check ownership |
| NFR-S4 | Players shall not receive the correctness of answers before the reveal. | Not implemented: live game state with correct/wrong flags is sent to all clients after every answer |
| NFR-S5 | Reading a user profile by ID shall not sign the caller in as that user. | Not implemented: `GET /api/users/:id` sets the session cookie for that ID |
| NFR-S6 | Users shall not be able to change or delete other users' accounts. | Not implemented |
| NFR-S7 | The API shall accept requests only from the frontend origin. | Implemented (CORS with `ALLOWED_ORIGIN`) |

## Performance and capacity

| ID | Requirement | Status |
|---|---|---|
| NFR-P1 | A host action (switch question, reveal) shall reach all players in under 1 second. | Implemented for small groups (single process, in-memory state) |
| NFR-P2 | The system shall support at least 50 players in one game. | Not verified |
| NFR-P3 | The system shall support several games at the same time. | Not implemented (BR-20) |
| NFR-P4 | REST requests shall complete within the client timeout of 1 second. | Partial: the 1 s timeout may be too short for a cold container start |

## Reliability and data

| ID | Requirement | Status |
|---|---|---|
| NFR-R1 | A live game shall survive a short disconnect of a player or the host (reconnect within 30 s). | Not implemented |
| NFR-R2 | A server restart shall not lose quizzes. | Implemented (SQLite file) |
| NFR-R3 | Deleting a quiz shall delete its questions and accepted answers. | Not implemented: no foreign keys or cascade |
| NFR-R4 | Game results shall be saved. | Not implemented |

## Usability

| ID | Requirement | Status |
|---|---|---|
| NFR-U1 | The player screen shall be usable on a phone. | Not verified |
| NFR-U2 | Errors from the server shall be shown as readable messages. | Partial: server error texts are shown in toasts as-is (for example `Error: ...`) |
| NFR-U3 | Players shall be able to join by a link, without typing the ID. | Not implemented: the "link" is the bare game ID |
| NFR-U4 | Quizzes shall be shown by title, not by number. | Not implemented |

## Operations

| ID | Requirement | Status |
|---|---|---|
| NFR-O1 | Frontend is deployed as a static site to GitHub Pages. | Implemented |
| NFR-O2 | Backend is deployed as a Docker container (Amvera) with database migrations. | Implemented |
| NFR-O3 | Automated tests and CI shall run on every PR. | Not implemented |
