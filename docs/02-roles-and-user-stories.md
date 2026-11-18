# 2. Roles and user stories

## Roles

| Role | Who | What they can do |
|---|---|---|
| Guest | Not signed in | Sign in (or sign up automatically) |
| User | Signed in | See own quizzes, create a quiz, join a running game |
| Host | User who owns a quiz | Edit the quiz, start it, run the live game |
| Player | User who joined a running game | Answer questions, see other players and the winners |

Roles are not stored in the database: a user is a host for the quizzes they created (`Game.adminId`) and a player while connected to someone else's running game.

## User stories

Acceptance criteria describe the current behavior. Status values are explained in the [README](../README.md#status-legend).

### Account

**US-1. Sign in or sign up in one step** (Implemented)
As a guest, I want to enter a nickname and a password and get in, so that I do not need a separate registration form.
- Given a nickname and password of an existing user, when I press "Enter", then I am signed in and see the main page.
- Given a nickname that does not exist, when I press "Enter", then a new account is created with these credentials and I am signed in.
- Given an existing nickname and a wrong password, then a new account cannot be created (nickname is taken) and I see an error message.
- Both fields are required.

**US-2. Stay signed in** (Implemented)
As a user, I want the app to remember me, so that I do not sign in on every visit.
- When I open the start page and my saved user still exists, I am redirected to the main page.

**US-3. Log out** (Implemented)
As a user, I want to log out, so that someone else can use this device.
- When I press "Logout", my saved session is removed and I return to the sign-in page.

### Host: building a quiz

**US-4. See my quizzes** (Implemented)
As a host, I want to see the list of quizzes I created, so that I can open one of them.
- The main page shows my quizzes as "Quiz #1", "Quiz #2", ... in creation order.
- If I have no quizzes, I see "You haven't created any games yet."

**US-5. Create a quiz** (Implemented)
As a host, I want to create a new quiz in one click and go straight to the builder.

**US-6. Add and edit questions** (Implemented)
As a host, I want to add questions with text, an optional picture and accepted answers, so that players have something to answer.
- "Add new question" creates an empty question and opens it in the editor.
- Question text is required; the image is set by URL and previewed.
- At least one accepted answer is required; empty accepted answers are not allowed.
- I can add and remove accepted answers.
- Saving replaces the stored accepted answers with the ones in the form; I see a success message.

**US-7. Delete a question** (Implemented)
As a host, I want to delete a question together with its accepted answers.

**US-8. Start a quiz only when it is ready** (Partial)
As a host, I want the "Start this quiz" button to be available only when every question has text and at least one accepted answer.
- Current behavior: the button is always enabled ([GAP-6](08-gaps-and-backlog.md)).

### Host: running a live game

**US-9. Start a live game** (Partial)
As a host, I want to start my quiz live and share its ID with players.
- The first question is active when the game starts.
- "Copy link for the players" copies the **game ID** (not a URL) to the clipboard and shows a confirmation for 3 seconds.
- Limitation: only one live game can run on the server at a time ([GAP-1](08-gaps-and-backlog.md)).

**US-10. See who joined and who answered** (Implemented)
As a host, I want to see the list of players and who has already answered the current question.
- Each player has a random color; players who answered the current question are highlighted.

**US-11. Switch questions** (Implemented)
As a host, I want to choose which question is shown to players, in any order.

**US-12. Reveal answers** (Implemented)
As a host, I want to show every player's answer to the current question, marked correct (green) or wrong (red).
- Players who did not answer are shown with "No answer" and counted as wrong.

**US-13. Show winners and end the game** (Implemented)
As a host, I want to end the quiz and show the winners table to everyone, then stop the game.
- "End Quiz and show winners" shows the winners table to the host and redirects players to the winners page.
- "Stop Quiz" becomes available after the winners are shown and ends the game on the server.
- If the host closes the page, the game ends.

### Player

**US-14. Join a game by ID** (Implemented)
As a player, I want to enter a game ID and join the running game.
- If no game is running, I see "There is no active game at the moment".
- If another game is running, I see "No game with id … is currently running".
- Joining twice with the same account does not create a duplicate player.

**US-15. Answer the current question** (Partial)
As a player, I want to type my answer and send it.
- Empty answers are not allowed.
- The screen says the answer cannot be changed after sending.
- Limitation: the input is locked only after the host switches questions; before that a player can send several answers ([GAP-5](08-gaps-and-backlog.md)).

**US-16. See results** (Implemented)
As a player, I want to see the revealed answers of all players and the final winners table.
