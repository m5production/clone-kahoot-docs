# 1. Product vision

## Problem

Teachers, event hosts and team leads want to run a short live quiz for a group of people: everyone answers on their own device, the host controls the pace, and the results are shown at the end. Existing tools are often paid, heavy, or require players to install an app.

## Product

Clone-Kahoot is a web app with two sides:

- **Host side:** create a quiz (a set of questions with accepted answers and optional images), start it live, switch questions, reveal answers and show the winners.
- **Player side:** join a running quiz by its game ID, answer each question by typing text, and see the final leaderboard.

Everything runs in the browser. Players and the host see changes in real time over WebSocket.

## Goals

| ID | Goal | How we measure it |
|---|---|---|
| G-1 | A host can prepare a quiz without help | Time from sign-in to a ready quiz with 5 questions |
| G-2 | Players join in seconds | Time from receiving the game ID to seeing the first question |
| G-3 | The game feels live | Delay between a host action and its display on player screens |
| G-4 | Results are fair and clear | Winners table matches the number of correct answers of each player |

## Target users

- **Host (quiz author):** creates and runs quizzes. Typical cases: a teacher in a class, a speaker at a meetup, a team lead at a team event.
- **Player (participant):** joins a quiz from a laptop or phone and answers questions.

The same account can act as a host for its own quizzes and as a player in other quizzes.

## Scope

### In scope (current product)

- Sign-in with nickname and password; the account is created automatically on the first sign-in
- List of the host's quizzes and creation of a new quiz
- Quiz builder: questions with text, optional image URL and one or more accepted text answers
- Live game run by the host: question switching, answer reveal, winners table, game end
- Player screen: current question, answer input, list of players, winners table

### Out of scope (current product)

- Multiple-choice answers with colored buttons (the original Kahoot format); answers are free text
- Timer per question and speed-based scoring
- Several live games at the same time (see [GAP-1](08-gaps-and-backlog.md))
- Quiz titles, descriptions, categories, sharing and public quiz library
- Analytics and history of past games

## Assumptions and constraints

- A game is small: one host and a classroom-sized group of players.
- The host and players are online during the whole game; live game state is kept in server memory only.
- Frontend is a static SPA on GitHub Pages; backend is a single container with an SQLite database.
