# 3. Functional requirements

Each requirement links to the code that implements it. "FE" is the [frontend](https://github.com/m5production/clone-kahoot), "BE" is the [backend](https://github.com/m5production/clone-kahoot-backend).

## 3.1 Authentication and account

| ID | Requirement | Status | Code |
|---|---|---|---|
| FR-1 | The system shall let a guest sign in with a nickname and a password. | Implemented | FE [Authorisation](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Authorisation/index.tsx), BE [userController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/userController.ts) |
| FR-2 | If the nickname is not registered, the system shall create a new account with the entered credentials and sign the user in. | Implemented | FE [useUserAuthorize](https://github.com/m5production/clone-kahoot/blob/HEAD/src/useUserAuthorize.ts) |
| FR-3 | Nicknames shall be unique. | Implemented | BE [schema](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/prisma/schema.prisma) |
| FR-4 | The system shall keep the user signed in between visits and redirect a signed-in user from the sign-in page to the main page. | Implemented | FE [useUserAuthorize](https://github.com/m5production/clone-kahoot/blob/HEAD/src/useUserAuthorize.ts) |
| FR-5 | Pages other than sign-in shall be available only to signed-in users. | Partial: the check happens in the browser only (see [NFR-S2](07-non-functional-requirements.md)) | FE [Router](https://github.com/m5production/clone-kahoot/blob/HEAD/src/Router.tsx) |
| FR-6 | The user shall be able to log out. | Implemented | FE [MainWindow](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/MainWindow/index.tsx) |
| FR-7 | The main page shall show the nickname of the signed-in user. | Implemented | FE [UserAvatar](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/UserAvatar.tsx) |

## 3.2 Quiz management

| ID | Requirement | Status | Code |
|---|---|---|---|
| FR-10 | The system shall show the list of quizzes created by the signed-in user. | Implemented | BE [gameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/gameController.ts) |
| FR-11 | The user shall be able to create a new quiz; the creator becomes its host. | Implemented | FE [usePostAddNewGame](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/MainWindow/api/usePostAddNewGame.ts) |
| FR-12 | The host shall be able to give the quiz a title. | Not implemented: quizzes are shown as "Quiz #N" | |
| FR-13 | The host shall be able to delete a quiz with all its questions. | Not implemented in UI; the API deletes the quiz record only | BE [gameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/gameController.ts) |

## 3.3 Quiz builder

| ID | Requirement | Status | Code |
|---|---|---|---|
| FR-20 | The builder shall list the questions of the quiz as "Question 1", "Question 2", ... | Implemented | FE [QuestionsList](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionsList.tsx) |
| FR-21 | The host shall be able to add a new (empty) question; it opens in the editor. | Implemented | FE [QuestionsList](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionsList.tsx) |
| FR-22 | A question shall have required text and an optional image given by URL, with a preview. | Implemented | FE [QuestionConstructor](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionConstructor.tsx) |
| FR-23 | A question shall have one or more accepted answers; the host can add and remove them. | Implemented | FE [QuestionConstructor](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionConstructor.tsx), BE [acceptedAnswersController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/acceptedAnswersController.ts) |
| FR-24 | Saving a question shall update its text and image and replace its accepted answers. | Implemented | BE [acceptedAnswersController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/acceptedAnswersController.ts) |
| FR-25 | The host shall be able to delete a question with its accepted answers. | Implemented | FE [useDeleteQuestion](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/api/useDeleteQuestion.ts) |
| FR-26 | The host shall be able to change the order of questions. | Not implemented: order is the database order | |
| FR-27 | "Start this quiz" shall be disabled until every question has text and at least one accepted answer. | Not implemented: button is always enabled | FE [AdminQuizReview](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/index.tsx) |
| FR-28 | Only the host of a quiz shall be able to view and edit it. | Not implemented: any user with the quiz ID can edit it | BE [questionController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/questionController.ts) |

## 3.4 Live game: host

| ID | Requirement | Status | Code |
|---|---|---|---|
| FR-30 | The host shall be able to start a live game from the builder; the first question becomes active. | Implemented | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
| FR-31 | The system shall reject starting a game while another game is running. | Implemented (limitation, see [GAP-1](08-gaps-and-backlog.md)) | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
| FR-32 | The host shall be able to copy the game ID to share with players. | Implemented | FE [ActiveQuiz](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminActiveQuiz/ActiveQuiz.tsx) |
| FR-33 | The host shall see the active question (text and image) and the list of players in real time. | Implemented | FE [ActiveQuiz](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminActiveQuiz/ActiveQuiz.tsx) |
| FR-34 | The host shall see which players have answered the active question. | Implemented | FE [Players](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/Players.tsx) |
| FR-35 | The host shall be able to make any question of the quiz active, in any order. | Implemented | |
| FR-36 | The host shall be able to reveal all answers to the active question, marked correct or wrong, to everyone. | Implemented | |
| FR-37 | The host shall be able to end the quiz and show the winners table to everyone. | Implemented | FE [WinnersTable](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/WinnersTable.tsx) |
| FR-38 | The host shall be able to stop the game after the winners are shown. | Implemented | |
| FR-39 | The game shall end when the host leaves the live game page. | Implemented | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |

## 3.5 Live game: player

| ID | Requirement | Status | Code |
|---|---|---|---|
| FR-40 | A signed-in user shall be able to join a running game by entering its ID. | Implemented | FE [MainWindow](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/MainWindow/index.tsx), BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
| FR-41 | The system shall show a clear message when the game ID is not running. | Implemented | |
| FR-42 | A user shall appear in the game only once, even after reconnecting. | Implemented | |
| FR-43 | Each player shall get a color for the players list. | Implemented: random from 6 colors | BE [utils](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/utils.ts) |
| FR-44 | The player shall see the active question and change it when the host switches questions. | Implemented | FE [Player](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Player/index.tsx) |
| FR-45 | The player shall send one non-empty text answer per question and shall not be able to change it. | Partial: see [GAP-5](08-gaps-and-backlog.md) | FE [Player](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Player/index.tsx) |
| FR-46 | The player shall see the revealed answers of all players. | Implemented | FE [Players](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/Players.tsx) |
| FR-47 | The player shall be taken to the winners page when the host shows the winners. | Implemented | FE [Winners](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Winners/index.tsx) |
| FR-48 | The player shall be told when the game is ended by the host. | Not implemented: the player screen ignores the end-of-game event | FE [ActiveGameProvider](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/ActiveGameProvider.tsx) |
| FR-49 | A player who disconnects and returns shall keep their answers. | Not implemented: a disconnected player is removed with all answers | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
