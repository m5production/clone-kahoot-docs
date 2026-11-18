# 8. Gaps and backlog

Gaps found while comparing the code with the requirements in this repository. Priorities:

- **P0:** breaks fairness or security; fix before showing the product to real users.
- **P1:** core user experience.
- **P2:** quality and technical debt.
- **P3:** new features.

## Gaps

| ID | Priority | Gap | Impact | Related |
|---|---|---|---|---|
| GAP-2 | P0 | Passwords are stored in plain text; the session cookie is the plain user ID; `GET /api/users/:id` signs the caller in as that user. | Anyone who learns a user ID (it is sent to all players in the live game) can act as that user. | NFR-S1, NFR-S2, NFR-S5 |
| GAP-3 | P0 | No ownership checks: any user can edit or delete any quiz, question, accepted answer or account; any WebSocket client can send host messages (switch question, reveal, end game). | A player can end or disrupt someone else's game. | FR-28, NFR-S3, NFR-S6 |
| GAP-4 | P0 | After every answer the server sends the full live state, including each player's correct/wrong flags, to all clients. | Players can see in the browser dev tools who answered correctly before the reveal. | BR-34, NFR-S4 |
| GAP-5 | P0 | A player can send several answers to the same question until the host switches questions; the server stores all of them and counts every correct one. | A player can raise their score by sending the correct answer several times. | BR-31, BR-35, FR-45 |
| GAP-1 | P1 | Only one live game per server; all live messages go to every connected client. | Two hosts cannot run quizzes at the same time. | BR-20, NFR-P3 |
| GAP-6 | P1 | "Add new question" saves an empty question at once; "Start this quiz" is always enabled. | A quiz can be started with empty questions or questions without accepted answers. | FR-27, US-8 |
| GAP-7 | P1 | Answer check is exact and case-sensitive. | `paris` is wrong when the accepted answer is `Paris`; hosts must list every spelling. | BR-32 |
| GAP-8 | P1 | The player screen ignores `END_GAME` and `FAULT` messages. | Players are not told that the game ended or that an action failed. | FR-48 |
| GAP-9 | P1 | A disconnected player is removed with all answers; a host disconnect ends the game. | A page reload or a weak mobile network loses the player's progress. | BR-25, NFR-R1, FR-49 |
| GAP-10 | P1 | Quizzes have no title; the list shows "Quiz #1", "Quiz #2". | Hosts with several quizzes cannot tell them apart. | FR-12, NFR-U4 |
| GAP-11 | P1 | "Copy link for the players" copies the bare game ID; players must type it into the main page. | Joining is slower and error-prone, especially on phones. | NFR-U3 |
| GAP-12 | P2 | No UI to delete a quiz; the API deletes the quiz record but leaves its questions and answers. | Orphaned data in the database. | FR-13, NFR-R3 |
| GAP-13 | P2 | Question order is not defined (no explicit order field) and cannot be changed. | The order of questions may differ from what the host expects. | FR-26, BR-11 |
| GAP-14 | P2 | The API returns `401` or `404` for validation errors and sends raw `Error: ...` strings. | Confusing error messages; hard to handle errors on the frontend. | NFR-U2 |
| GAP-15 | P2 | Tied players have no defined place in the winners table. | Results may look unfair. | BR-36 |
| GAP-16 | P2 | No automated tests or CI in either repository. | Regressions are found only manually. | NFR-O3 |
| GAP-17 | P2 | Game results are not saved. | The host cannot review or export results after the game. | BR-26, NFR-R4 |

## Prioritized backlog

### P0: fairness and security

1. Hash passwords; replace the user-ID cookie with a random server-side session or a signed `HttpOnly` token; stop setting the cookie in `GET /api/users/:id`. (GAP-2)
2. Check on the server that the caller owns the quiz for every write request and every host WebSocket message. (GAP-3)
3. Send players only public game state: players, who answered (yes/no), active question. Send correctness only in the reveal. (GAP-4)
4. Accept one answer per player per question on the server; lock the input on the client right after sending. (GAP-5)

### P1: core experience

5. Support several live games: keep a map of games by ID and send messages only to clients of that game. (GAP-1)
6. Validate the quiz before start: every question has text and at least one accepted answer. (GAP-6)
7. Normalize answers before comparison: trim spaces, ignore case. Confirm with the product owner first. (GAP-7)
8. Handle `END_GAME` and `FAULT` on the player screen. (GAP-8)
9. Keep player state for a short time after a disconnect and restore it on reconnect. (GAP-9)
10. Add quiz titles. (GAP-10)
11. Share a join URL (and a QR code) that opens the game directly. (GAP-11)

### P2: quality

12. Delete a quiz from the UI with all its questions and answers; add foreign keys with cascade delete. (GAP-12)
13. Add an explicit question order and drag-and-drop reordering. (GAP-13)
14. Use correct HTTP status codes and a JSON error format. (GAP-14)
15. Show tied players on the same place. (GAP-15)
16. Add unit tests for scoring and answer checks, an end-to-end test for a full game, and CI. (GAP-16)
17. Save game results and show a history of past games to the host. (GAP-17)

### P3: closer to Kahoot

18. Multiple-choice questions with 2 to 4 options.
19. Timer per question; time-based score bonus.
20. Leaderboard after each question.
21. Join as a guest with a nickname only, without an account.
22. Public library of quizzes and quiz sharing between hosts.
