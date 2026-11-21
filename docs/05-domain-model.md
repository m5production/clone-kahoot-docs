# 5. Доменная модель и глоссарий

## Хранимые сущности

Эти сущности хранятся в базе данных (PostgreSQL через Prisma). Соответствует целевой модели из [раздела 9.7](09-target-requirements.md#97-целевая-доменная-модель) плюс `Session` (сессия входа) и `QuestionOption` ([документ 13](13-multiple-choice-questions.md)).

```mermaid
erDiagram
    User ||--o{ Session : "сессии входа (userId)"
    User ||--o{ Game : "ведёт (adminId)"
    User ||--o{ GameSession : "проводит (hostId)"
    User ||--o{ SessionPlayer : "играет как (userId)"
    Game ||--o{ Question : "содержит (gameId)"
    Game ||--o{ GameSession : "запускается в (gameId)"
    Question ||--|{ AcceptedAnswer : "принимает (questionId)"
    Question ||--o{ QuestionOption : "варианты (questionId)"
    GameSession ||--|{ SessionQuestion : "копия вопросов (sessionId)"
    GameSession ||--o{ SessionPlayer : "участники (sessionId)"
    SessionPlayer ||--o{ SessionAnswer : "отвечает (sessionPlayerId)"
    SessionQuestion ||--o{ SessionAnswer : "получает (sessionQuestionId)"

    User {
        string id
        string name "уникальный никнейм"
        string passwordHash
    }
    Session {
        string id
        string tokenHash "хеш токена входа"
        string userId
        datetime expiresAt "30 дней (BR-42)"
    }
    Game {
        string id
        string adminId "id ведущего"
        string title "1–80 символов (BR-44)"
        datetime createdAt
        datetime updatedAt "сортировка списка (BR-45)"
    }
    Question {
        string id
        string gameId
        int order "порядок, меняет ведущий (BR-11)"
        string type "'open' | 'choice', по умолчанию 'open'"
        string text "1–300 символов (BR-46)"
        string img "необязательная ссылка на картинку"
        int timeLimitSec "10–120 с, по умолчанию 20 (BR-48)"
    }
    AcceptedAnswer {
        string id
        string questionId
        string text "1–100 символов (BR-47)"
    }
    QuestionOption {
        string id
        string questionId
        int order "порядок показа (D-13)"
        string text "≤ 100 символов"
        boolean isCorrect "ровно один верный"
    }
    GameSession {
        string id "id сеанса из ссылки /join/<id>"
        string gameId "SetNull при удалении квиза"
        string hostId
        string quizTitle "название на момент игры"
        string status "running | finished | aborted"
        datetime startedAt
        datetime endedAt
    }
    SessionQuestion {
        string id
        string sessionId
        string text
        string img
        int timeLimitSec
        string type "'open' | 'choice'"
        json acceptedAnswers "копия на момент игры"
        json options "снапшот вариантов для choice"
    }
    SessionPlayer {
        string id
        string sessionId
        string userId
        string nickname
        string color
        int score
        int correctCount
        int correctTimeMs "тай-брейк BR-72"
        int place
    }
    SessionAnswer {
        string id
        string sessionPlayerId
        string sessionQuestionId
        string text "текст или выбранный вариант"
        boolean isCorrect
        int responseTimeMs
        int points "по формуле BR-70"
    }
```

Связи — настоящие внешние ключи. Удаление квиза каскадно удаляет вопросы, принимаемые ответы и варианты (`onDelete: Cascade`), но не историю: `GameSession.gameId` становится `NULL` (`onDelete: SetNull`), а копия вопросов (`SessionQuestion`) и название квиза (`quizTitle`) сохраняются (BR-81). Сеанс и всё его содержимое удаляются каскадом по `sessionId` (BR-82).

## Сущности живой игры (в памяти)

Состояние идущего сеанса хранится в памяти сервера — комнаты в `Map<sessionId, Room>` на одном экземпляре (`ActiveGameService`). Результаты записываются в базу при закрытии каждого вопроса и при завершении сеанса (NFR-R6); при перезапуске сервера идущие сеансы прерываются и попадают в историю со статусом «прервана» (NFR-R7).

| Сущность | Поля | Примечания |
|---|---|---|
| Сеанс игры (комната) | `sessionId`, `gameId`, название квиза, `hostId`, состояние (лобби → вопрос → ответы → победители), номер текущего вопроса, снапшот вопросов с принимаемыми ответами и вариантами, игроки, сокеты, таймер вопроса, grace-таймер ведущего | Одновременно много комнат (BR-50); события рассылаются только участникам комнаты |
| Игрок в сеансе | id пользователя, никнейм, цвет, «в сети», «ответил», ответы и очки | Отключение не удаляет игрока — статус «не в сети» до конца (BR-58) |
| Ответ игрока | текст или `optionId`, правильность, время ответа, очки | Первый ответ считается (BR-63); правильность вычисляется при получении |

## Глоссарий

| Термин | Значение | Название в коде |
|---|---|---|
| Квиз | Набор вопросов, подготовленный ведущим. В коде называется «game». | `Game` |
| Ведущий | Пользователь, который создал квиз и проводит его вживую | `adminId`, `hostId`, «admin» в маршрутах |
| Сессия входа | Долгоживущая сессия аккаунта (30 дней): токен в localStorage клиента, в базе — его хеш | `Session`, `Authorization: Bearer` |
| Сеанс игры | Один запуск квиза вживую со своей ссылкой-приглашением; запись о нём — история игр | `GameSession`, `sessionId`, комната `Room` |
| Ссылка-приглашение | URL, по которому игроки подключаются к сеансу; QR-код кодирует её | `/join/<sessionId>`, `joinPath` |
| Вопрос | Задание с текстом и необязательной картинкой. Тип `open` — ответ текстом, `choice` — выбор из вариантов | `Question` |
| Принимаемый ответ | Текст, который считается правильным ответом на вопрос `open`. У вопроса их может быть несколько. | `AcceptedAnswer` |
| Вариант ответа | Один из 2–4 предложенных ответов вопроса `choice`; верным помечен ровно один ([документ 13](13-multiple-choice-questions.md)) | `QuestionOption` |
| Текущий вопрос | Вопрос, который игроки видят и на который отвечают сейчас | `question` в `SESSION_STATE` |
| Показ ответов | Показ ответов всех игроков на текущий вопрос с правильностью и очками | `ANSWERS_REVEALED` |
| Очки | Очки за правильные ответы с бонусом за скорость по формуле BR-70 | `score`, `points` |
| Таблица победителей | Итоговый рейтинг по очкам с тай-брейком по времени (BR-72) | `WINNERS`, `WinnersTable` |
| История игр | Сохранённые записи о прошлых сеансах у ведущего | `GameSession` + `Session*` |
