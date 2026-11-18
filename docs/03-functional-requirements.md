# 3. Функциональные требования

Требования версии 1.0 (FR-50–FR-83) и изменения требований ниже — в [целевых требованиях](09-target-requirements.md#96-новые-функциональные-требования).

Каждое требование ссылается на код, который его реализует. «FE» — [фронтенд](https://github.com/m5production/clone-kahoot), «BE» — [бэкенд](https://github.com/m5production/clone-kahoot-backend).

## 3.1 Вход и аккаунт

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-1 | Система должна позволять гостю войти по никнейму и паролю. | Реализовано | FE [Authorisation](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Authorisation/index.tsx), BE [userController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/userController.ts) |
| FR-2 | Если никнейм не зарегистрирован, система должна создать новый аккаунт с введёнными данными и выполнить вход. | Реализовано | FE [useUserAuthorize](https://github.com/m5production/clone-kahoot/blob/HEAD/src/useUserAuthorize.ts) |
| FR-3 | Никнеймы должны быть уникальными. | Реализовано | BE [schema](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/prisma/schema.prisma) |
| FR-4 | Система должна сохранять вход между визитами и перенаправлять вошедшего пользователя со страницы входа на главную. | Реализовано | FE [useUserAuthorize](https://github.com/m5production/clone-kahoot/blob/HEAD/src/useUserAuthorize.ts) |
| FR-5 | Все страницы, кроме страницы входа, должны быть доступны только вошедшим пользователям. | Частично: проверка только в браузере (см. [NFR-S2](07-non-functional-requirements.md)) | FE [Router](https://github.com/m5production/clone-kahoot/blob/HEAD/src/Router.tsx) |
| FR-6 | Пользователь должен иметь возможность выйти. | Реализовано | FE [MainWindow](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/MainWindow/index.tsx) |
| FR-7 | На главной странице должен показываться никнейм вошедшего пользователя. | Реализовано | FE [UserAvatar](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/UserAvatar.tsx) |

## 3.2 Управление квизами

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-10 | Система должна показывать список квизов, созданных вошедшим пользователем. | Реализовано | BE [gameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/gameController.ts) |
| FR-11 | Пользователь должен иметь возможность создать новый квиз; создатель становится его ведущим. | Реализовано | FE [usePostAddNewGame](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/MainWindow/api/usePostAddNewGame.ts) |
| FR-12 | Ведущий должен иметь возможность задать квизу название. | Не реализовано: квизы показаны как «Quiz #N» | |
| FR-13 | Ведущий должен иметь возможность удалить квиз со всеми вопросами. | Не реализовано в интерфейсе; API удаляет только запись квиза | BE [gameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/gameController.ts) |

## 3.3 Конструктор квиза

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-20 | Конструктор должен показывать вопросы квиза списком «Question 1», «Question 2», ... | Реализовано | FE [QuestionsList](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionsList.tsx) |
| FR-21 | Ведущий должен иметь возможность добавить новый (пустой) вопрос; он открывается в редакторе. | Реализовано | FE [QuestionsList](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionsList.tsx) |
| FR-22 | У вопроса должен быть обязательный текст и необязательная картинка по ссылке с превью. | Реализовано | FE [QuestionConstructor](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionConstructor.tsx) |
| FR-23 | У вопроса должен быть один или несколько принимаемых ответов; ведущий может их добавлять и удалять. | Реализовано | FE [QuestionConstructor](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/QuestionConstructor.tsx), BE [acceptedAnswersController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/acceptedAnswersController.ts) |
| FR-24 | Сохранение вопроса должно обновлять его текст и картинку и заменять его принимаемые ответы. | Реализовано | BE [acceptedAnswersController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/acceptedAnswersController.ts) |
| FR-25 | Ведущий должен иметь возможность удалить вопрос вместе с принимаемыми ответами. | Реализовано | FE [useDeleteQuestion](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/api/useDeleteQuestion.ts) |
| FR-26 | Ведущий должен иметь возможность менять порядок вопросов. | Не реализовано: порядок определяется базой данных | |
| FR-27 | Кнопка «Start this quiz» должна быть недоступна, пока у каждого вопроса нет текста и хотя бы одного принимаемого ответа. | Не реализовано: кнопка доступна всегда | FE [AdminQuizReview](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminQuizReview/index.tsx) |
| FR-28 | Просматривать и редактировать квиз должен только его ведущий. | Не реализовано: любой пользователь, знающий ID квиза, может его редактировать | BE [questionController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/questionController.ts) |

## 3.4 Живая игра: ведущий

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-30 | Ведущий должен иметь возможность запустить живую игру из конструктора; активным становится первый вопрос. | Реализовано | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
| FR-31 | Система должна отклонять запуск игры, пока идёт другая игра. | Реализовано (это ограничение, см. [GAP-1](08-gaps-and-backlog.md)) | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
| FR-32 | Ведущий должен иметь возможность скопировать ID игры, чтобы отправить его игрокам. | Реализовано | FE [ActiveQuiz](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminActiveQuiz/ActiveQuiz.tsx) |
| FR-33 | Ведущий должен видеть активный вопрос (текст и картинку) и список игроков в реальном времени. | Реализовано | FE [ActiveQuiz](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/AdminActiveQuiz/ActiveQuiz.tsx) |
| FR-34 | Ведущий должен видеть, кто из игроков ответил на активный вопрос. | Реализовано | FE [Players](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/Players.tsx) |
| FR-35 | Ведущий должен иметь возможность сделать активным любой вопрос квиза в любом порядке. | Реализовано | |
| FR-36 | Ведущий должен иметь возможность показать всем все ответы на активный вопрос с отметкой «верно» или «неверно». | Реализовано | |
| FR-37 | Ведущий должен иметь возможность завершить квиз и показать всем таблицу победителей. | Реализовано | FE [WinnersTable](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/WinnersTable.tsx) |
| FR-38 | Ведущий должен иметь возможность остановить игру после показа победителей. | Реализовано | |
| FR-39 | Игра должна заканчиваться, когда ведущий уходит со страницы живой игры. | Реализовано | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |

## 3.5 Живая игра: игрок

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-40 | Вошедший пользователь должен иметь возможность подключиться к идущей игре, введя её ID. | Реализовано | FE [MainWindow](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/MainWindow/index.tsx), BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
| FR-41 | Если игра с таким ID не идёт, система должна показывать понятное сообщение. | Реализовано | |
| FR-42 | Пользователь должен появляться в игре только один раз, даже после переподключения. | Реализовано | |
| FR-43 | Каждому игроку должен назначаться цвет в списке игроков. | Реализовано: случайный из 6 цветов | BE [utils](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/utils.ts) |
| FR-44 | Игрок должен видеть активный вопрос, и вопрос должен меняться, когда ведущий его переключает. | Реализовано | FE [Player](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Player/index.tsx) |
| FR-45 | Игрок должен отправлять один непустой текстовый ответ на вопрос и не иметь возможности его изменить. | Частично: см. [GAP-5](08-gaps-and-backlog.md) | FE [Player](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Player/index.tsx) |
| FR-46 | Игрок должен видеть показанные ответы всех игроков. | Реализовано | FE [Players](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/Players.tsx) |
| FR-47 | Когда ведущий показывает победителей, игрок должен попадать на страницу победителей. | Реализовано | FE [Winners](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/Winners/index.tsx) |
| FR-48 | Игрок должен узнавать, что ведущий завершил игру. | Не реализовано: экран игрока игнорирует событие завершения игры | FE [ActiveGameProvider](https://github.com/m5production/clone-kahoot/blob/HEAD/src/components/ui/ActiveGameProvider.tsx) |
| FR-49 | Игрок, который отключился и вернулся, должен сохранять свои ответы. | Не реализовано: отключившийся игрок удаляется вместе со всеми ответами | BE [activeGameController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/controllers/activeGameController.ts) |
