## Список сервисов

1. **TalentService** — актёры, режиссёры, сценаристы; хранит базовые характеристики и статус `FREE/BUSY`.
2. **StudioService** — киностудии игроков и ИИ‑соперников; контракты, финансы, найм/увольнение.
3. **ProductionService** — черновик фильма, сценарий, подбор каста, процесс съёмок.
4. **MarketingService** — маркетинговые кампании **до** и **после** релиза, расходы и эффективность.
5. **ReleaseService** — выпуск фильма в прокат, сбор кассы, популярность.
6. **AwardService** — Оскар: расписание наград, определение лауреатов.
7. **RatingService** — агрегированные рейтинги фильмов, актёров, студий; публикация `RatingUpdatedEvent`.

---

## Цепочка команд и событий

| №  | Команда / событие           | Инициатор (кто посылает) | Владелец логики (сервис) | Опубликованное событие          | Подписчики (кто слушает)                        |
|----|-----------------------------|--------------------------|--------------------------|---------------------------------|-------------------------------------------------|
| 1  | Нанять сотрудника           | UI → StudioService       | StudioService            | `SpecialistHiredEvent`          | TalentService, RatingService                    |
| 2  | Создать черновик фильма     | UI → ProductionService   | ProductionService        | `FilmDraftCreatedEvent`         | StudioService                                   |
| 3  | Назначить сценариста        | UI → ProductionService   | ProductionService        | `ScriptWriterAssignedEvent`     | TalentService                                   |
| 4  | Подтвердить каст            | UI → ProductionService   | ProductionService        | `CastConfirmedEvent`            | TalentService                                   |
| 5  | Запустить маркетинг         | UI → MarketingService    | MarketingService         | `MarketingCampaignStartedEvent` | ProductionService, RatingService, StudioService |
| 6  | Начать съёмки               | UI → ProductionService   | ProductionService        | `FilmShootingStartedEvent`      | StudioService                                   |
| 7  | Завершить съёмки            | ProductionService        | ProductionService        | `FilmFinishedEvent`             | ReleaseService                                  |
| 8  | Выпустить фильм в прокат    | UI → ReleaseService      | ReleaseService           | `FilmReleasedEvent`             | MarketingService, RatingService                 |
| 9  | Получен Оскар (крон)        | AwardService             | AwardService             | `OscarGrantedEvent`             | RatingService, TalentService                    |
| 10 | Пересчитать рейтинги (крон) | RatingService            | RatingService            | `RatingUpdatedEvent`            | StudioService, TalentService                    |
| 11 | Уволить сотрудника          | UI → StudioService       | StudioService            | `SpecialistReleasedEvent`       | TalentService, RatingService                    |