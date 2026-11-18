# 4. Business rules

## Accounts

| ID | Rule |
|---|---|
| BR-1 | A nickname identifies a user and must be unique across the system. |
| BR-2 | Signing in with an unknown nickname creates a new account with that nickname and password. |
| BR-3 | Signing in with a known nickname and a wrong password is rejected (the nickname is taken). |
| BR-4 | Any user can be a host and a player; there is no separate admin account. |

## Quizzes and questions

| ID | Rule |
|---|---|
| BR-10 | A quiz belongs to the user who created it (the host). |
| BR-11 | A quiz consists of zero or more questions. Questions are shown in the order they are stored. |
| BR-12 | A question has text (required to save), an optional image URL and at least one accepted answer. |
| BR-13 | Accepted answers of a question are saved as a whole set: saving replaces all previous accepted answers. |
| BR-14 | Deleting a question deletes its accepted answers. |

## Live game

| ID | Rule |
|---|---|
| BR-20 | Only one live game can run on the server at a time. Starting a second game is rejected with "The game is already running". |
| BR-21 | When a live game starts, the first question of the quiz is active. |
| BR-22 | The host decides which question is active and can return to earlier questions. |
| BR-23 | Players can join at any time while the game is running, including after the first questions. |
| BR-24 | A user is added to a game once; a repeated join is ignored. |
| BR-25 | A player who disconnects is removed from the game together with their answers. |
| BR-26 | When the host disconnects or stops the game, the game ends for everyone and its live state is discarded. Results are not saved. |

## Answers and scoring

| ID | Rule |
|---|---|
| BR-30 | A player answers by typing free text. An empty answer is not accepted. |
| BR-31 | A player gives one answer per question. The answer cannot be changed after sending. |
| BR-32 | An answer is **correct** when it exactly matches one of the accepted answers of the question. The match is case-sensitive and sensitive to spaces: `Paris`, `paris` and `Paris ` are three different answers. |
| BR-33 | A player who did not answer a question gets it counted as wrong. |
| BR-34 | Answers to a question are revealed to everyone only when the host presses "Show answers". |
| BR-35 | The score of a player is the number of correct answers. There is no time bonus and no penalty for wrong answers. |
| BR-36 | The winners table lists players by score, highest first. Players with equal scores have no defined order. |

## Open questions for the product owner

These rules are implied by the code but should be confirmed:

1. Should answer matching ignore case and surrounding spaces (BR-32)? Most hosts expect `paris` to match `Paris`.
2. Should players who join late be able to answer earlier questions (BR-22, BR-23)?
3. Should the winners table show ties as the same place (BR-36)?
4. Should a player keep their answers after a short disconnect (BR-25)?
5. Should game results be saved for later review (BR-26)?
