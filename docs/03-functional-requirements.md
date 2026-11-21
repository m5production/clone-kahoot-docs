# 3. Функциональные требования

Требования версии 1.0 (FR-50–FR-87) и изменения требований ниже — в [целевых требованиях](09-target-requirements.md#96-новые-функциональные-требования). Требования FR-88–FR-92 перенесены сюда из [документа 11](11-mobile-quiz-editor.md) после реализации.

Каждое требование ссылается на код, который его реализует. «FE» — [фронтенд](https://github.com/m5production/clone-kahoot), «BE» — [бэкенд](https://github.com/m5production/clone-kahoot-backend).

## 3.1 Вход и аккаунт

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-1 | Система должна позволять гостю войти по никнейму и паролю на отдельной странице входа; аккаунт при этом не создаётся. | Реализовано | FE [LoginPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/login/login.ui.tsx), [LoginForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/session/login/login.ui.tsx), BE [AuthController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/auth/auth.controller.ts) |
| FR-2 | Система должна позволять гостю зарегистрировать аккаунт по никнейму и паролю на отдельной странице регистрации и сразу выполнить вход. | Реализовано | FE [RegisterPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/register/register.ui.tsx), [RegisterForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/session/register/register.ui.tsx), BE [AuthController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/auth/auth.controller.ts) |
| FR-3 | Никнеймы должны быть уникальными. | Реализовано | BE [schema](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/prisma/schema.prisma) |
| FR-4 | Система должна сохранять вход между визитами и перенаправлять вошедшего пользователя со страницы входа на главную. | Реализовано | FE [SessionService](https://github.com/m5production/clone-kahoot/blob/HEAD/src/entities/session/session.service.ts) |
| FR-5 | Без входа доступны приветственный экран и страницы входа и регистрации; все остальные страницы доступны только вошедшим пользователям. | Реализовано | FE [AppRouter](https://github.com/m5production/clone-kahoot/blob/HEAD/src/app/router/app-router.tsx), [RequireAuth](https://github.com/m5production/clone-kahoot/blob/HEAD/src/app/router/require-auth.tsx), [GuestOnly](https://github.com/m5production/clone-kahoot/blob/HEAD/src/app/router/guest-only.tsx) |
| FR-6 | Пользователь должен иметь возможность выйти. | Реализовано | FE [HomePage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/home/home.ui.tsx) |
| FR-7 | На главной странице должен показываться никнейм вошедшего пользователя. | Реализовано | FE [UserAvatar](https://github.com/m5production/clone-kahoot/blob/HEAD/src/entities/user/user.ui.avatar.tsx) |

## 3.2 Управление квизами

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-10 | Система должна показывать список квизов, созданных вошедшим пользователем. | Реализовано | BE [GamesController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/games/games.controller.ts) |
| FR-11 | Пользователь должен иметь возможность создать новый квиз; создатель становится его ведущим. | Реализовано | FE [CreateGameForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/game/create-game/create-game.ui.tsx) |
| FR-12 | Ведущий должен иметь возможность задать квизу название. | Реализовано | FE [CreateGameForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/game/create-game/create-game.ui.tsx), [RenameGameTitle](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/game/rename-game/rename-game.ui.tsx), BE [GamesController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/games/games.controller.ts) |
| FR-13 | Ведущий должен иметь возможность удалить квиз со всеми вопросами. | Реализовано | FE [DeleteGameButton](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/game/delete-game/delete-game.ui.tsx), BE [schema](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/prisma/schema.prisma) |

## 3.3 Конструктор квиза

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-20 | Конструктор должен показывать вопросы квиза списком «Question 1», «Question 2», ... | Реализовано | FE [QuestionsSidebar](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.questions-sidebar.tsx) |
| FR-21 | Ведущий должен иметь возможность добавить новый (пустой) вопрос; он открывается в редакторе. | Реализовано | FE [QuestionsSidebar](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.questions-sidebar.tsx) |
| FR-22 | У вопроса должен быть обязательный текст и необязательная картинка по ссылке с превью. | Реализовано | FE [EditQuestionForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/question/edit-question/edit-question.ui.tsx) |
| FR-23 | У вопроса должен быть один или несколько принимаемых ответов; ведущий может их добавлять и удалять. | Реализовано | FE [EditQuestionForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/question/edit-question/edit-question.ui.tsx), BE [AcceptedAnswersController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/accepted-answers/accepted-answers.controller.ts) |
| FR-24 | Сохранение вопроса должно обновлять его текст и картинку и заменять его принимаемые ответы. | Реализовано | BE [AcceptedAnswersController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/accepted-answers/accepted-answers.controller.ts) |
| FR-25 | Ведущий должен иметь возможность удалить вопрос вместе с принимаемыми ответами. | Реализовано | FE [DeleteQuestionButton](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/question/delete-question/delete-question.ui.tsx) |
| FR-26 | Ведущий должен иметь возможность менять порядок вопросов. | Реализовано | FE [MoveQuestionButtons](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/question/reorder-questions/reorder-questions.ui.tsx), BE [GamesController.reorder](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/games/games.controller.ts) |
| FR-27 | Кнопка «Start this quiz» должна быть недоступна, пока у каждого вопроса нет текста и хотя бы одного принимаемого ответа. | Реализовано | FE [QuizEditorPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.tsx), BE [ActiveGameService](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.service.ts) |
| FR-28 | Просматривать и редактировать квиз должен только его ведущий. | Реализовано: `getOwned` проверяет владельца во всех изменяющих запросах (GAP-3) | BE [QuestionsController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/questions/questions.controller.ts) |
| FR-88 | При ширине меньше 768 px конструктор показывает один раздел за раз — «Вопросы» или «Вопрос» — с закреплённой нижней панелью навигации; при 768 px и шире сохраняется двухколоночная раскладка (US-33). | Реализовано ([документ 11](11-mobile-quiz-editor.md)) | FE [QuizEditorPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.tsx), [QuizEditorMobile](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.mobile.tsx) |
| FR-89 | Выбор вопроса и добавление нового переключают на раздел «Вопрос»; удаление открытого вопроса возвращает на раздел «Вопросы» (US-33). | Реализовано | FE [QuizEditorPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.tsx) |
| FR-90 | На разделе «Вопросы» нижней панели показывается число неготовых к игре вопросов (FR-55, BR-49). | Реализовано | FE [EditorNavBar](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.mobile-nav.tsx) |
| FR-91 | «Start this quiz» на узком экране — FAB над нижней панелью, видимая в обоих разделах; при неготовом квизе приглушена, нажатие объясняет причину (BR-49). | Реализовано | FE [StartSessionFab](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/active-game/start-session/start-session.ui.fab.tsx) |
| FR-92 | Название квиза и ссылка «Back to quizzes» на узком экране находятся в шапке; название обрезается троеточием и остаётся доступным для переименования (US-17, FR-50). | Реализовано | FE [QuizEditorMobile](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/quiz-editor/quiz-editor.ui.mobile.tsx) |
| FR-98 | В конструкторе у вопроса есть переключатель типа «текстовый ответ» / «выбор из вариантов»; смена типа при непустом наборе другого типа требует подтверждения и удаляет этот набор (US-35, D-12). | Реализовано ([документ 13](13-multiple-choice-questions.md)) | FE [EditQuestionForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/question/edit-question/edit-question.ui.tsx) |
| FR-99 | Для вопроса «выбор из вариантов» форма показывает список из 2–4 вариантов с радиокнопкой «верный» — отметить можно только один; варианты добавляются и удаляются в пределах лимита (US-35). | Реализовано | FE [EditQuestionForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/question/edit-question/edit-question.ui.tsx) |
| FR-100 | Сохранение вопроса «выбор из вариантов» отклоняется с ошибкой у поля, если вариантов меньше 2 или не отмечен верный; дубли вариантов подсвечиваются (BR-86). | Реализовано | FE [EditQuestionForm](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/question/edit-question/edit-question.ui.tsx), BE [OptionsController](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/options/options.controller.ts) |

## 3.4 Живая игра: ведущий

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-30 | Ведущий должен иметь возможность запустить живую игру из конструктора; активным становится первый вопрос. | Реализовано | BE [ActiveGameGateway](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.gateway.ts) |
| FR-31 | Система должна отклонять запуск игры, пока идёт другая игра. | Реализовано (этап 4a: не более одного сеанса на квиз и на ведущего, BR-51) | BE [ActiveGameGateway](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.gateway.ts) |
| FR-32 | Ведущий должен иметь возможность скопировать ID игры, чтобы отправить его игрокам. | Реализовано (этап 4a: копируется ссылка-приглашение `/join/<sessionId>` и QR-код) | FE [SessionPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/session/session.ui.view.tsx) |
| FR-33 | Ведущий должен видеть активный вопрос (текст и картинку) и список игроков в реальном времени. | Реализовано | FE [SessionPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/session/session.ui.view.tsx) |
| FR-34 | Ведущий должен видеть, кто из игроков ответил на активный вопрос. | Реализовано | FE [Players](https://github.com/m5production/clone-kahoot/blob/HEAD/src/entities/player/player.ui.list.tsx) |
| FR-35 | Ведущий должен иметь возможность сделать активным любой вопрос квиза в любом порядке. | Заменено целевым BR-60 (этап 4a): строгий порядок, только следующий вопрос | |
| FR-36 | Ведущий должен иметь возможность показать всем все ответы на активный вопрос с отметкой «верно» или «неверно». | Реализовано | |
| FR-37 | Ведущий должен иметь возможность завершить квиз и показать всем таблицу победителей. | Реализовано | FE [WinnersTable](https://github.com/m5production/clone-kahoot/blob/HEAD/src/entities/player/player.ui.winners-table.tsx) |
| FR-38 | Ведущий должен иметь возможность остановить игру после показа победителей. | Реализовано | |
| FR-39 | Игра должна заканчиваться, когда ведущий уходит со страницы живой игры. | Заменено целевым BR-59 (этап 4a): сеанс живёт 5-минутный grace-таймер ведущего | BE [ActiveGameGateway](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.gateway.ts) |
| FR-103 | Отчёт игры показывает для вопроса «выбор из вариантов» варианты с отметкой верного и выбор каждого игрока (US-37, FR-81). | Реализовано ([документ 13](13-multiple-choice-questions.md)) | FE [SessionReportPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/session-report/session-report.ui.tsx) |

## 3.5 Живая игра: игрок

| ID | Требование | Статус | Код |
|---|---|---|---|
| FR-40 | Вошедший пользователь должен иметь возможность подключиться к идущей игре, введя её ID. | Реализовано (этап 4a: вход по ссылке-приглашению или ввод ссылки в форму) | FE [HomePage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/home/home.ui.tsx), BE [ActiveGameGateway](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.gateway.ts) |
| FR-41 | Если игра с таким ID не идёт, система должна показывать понятное сообщение. | Реализовано (этап 4a: экран «ссылка недействительна / игра закончилась», FR-73) | |
| FR-42 | Пользователь должен появляться в игре только один раз, даже после переподключения. | Реализовано | |
| FR-43 | Каждому игроку должен назначаться цвет в списке игроков. | Реализовано: случайный из 6 цветов | BE [ActiveGameService](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.service.ts) |
| FR-44 | Игрок должен видеть активный вопрос, и вопрос должен меняться, когда ведущий его переключает. | Реализовано | FE [SessionPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/session/session.ui.view.tsx) |
| FR-45 | Игрок должен отправлять один непустой текстовый ответ на вопрос и не иметь возможности его изменить. | Реализовано (этап 3: BR-63, BR-64) | FE [SessionPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/session/session.ui.view.tsx), BE [ActiveGameService](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.service.ts) |
| FR-46 | Игрок должен видеть показанные ответы всех игроков. | Реализовано | FE [Players](https://github.com/m5production/clone-kahoot/blob/HEAD/src/entities/player/player.ui.list.tsx) |
| FR-47 | Когда ведущий показывает победителей, игрок должен попадать на страницу победителей. | Реализовано | FE [SessionPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/session/session.ui.view.tsx) |
| FR-48 | Игрок должен узнавать, что ведущий завершил игру. | Реализовано (этап 4a: `SESSION_ENDED` + экран окончания, FR-79) | FE [ActiveGameProvider](https://github.com/m5production/clone-kahoot/blob/HEAD/src/entities/active-game/active-game.provider.tsx) |
| FR-49 | Игрок, который отключился и вернулся, должен сохранять свои ответы. | Реализовано (этап 4a: BR-58, переподключение `JOIN_SESSION`, FR-77) | BE [ActiveGameGateway](https://github.com/m5production/clone-kahoot-backend/blob/HEAD/src/modules/active-game/active-game.gateway.ts) |
| FR-101 | Во время вопроса «выбор из вариантов» игрок видит сетку цветных кнопок-вариантов (2 колонки на широком экране, цвет по индексу: красный, синий, жёлтый, зелёный); нажатие отправляет выбор и блокирует кнопки (US-36). | Реализовано ([документ 13](13-multiple-choice-questions.md)) | FE [AnswerOptionButtons](https://github.com/m5production/clone-kahoot/blob/HEAD/src/features/active-game/submit-answer/submit-answer.ui.options.tsx) |
| FR-102 | После показа ответов игрок и ведущий видят варианты с подсветкой верного и пометкой своего выбора; у ведущего — список «игрок → выбранный вариант» (US-37). | Реализовано | FE [SessionPage](https://github.com/m5production/clone-kahoot/blob/HEAD/src/pages/session/session.ui.view.tsx) |
