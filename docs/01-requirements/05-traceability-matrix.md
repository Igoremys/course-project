# Таблица трассировки требований

| Параметр | Значение |
|----------|----------|
| **Проект** | PhotoFolio — мобильное приложение для создания фотопортфолио |
| **Траектория** | В (Мобильная разработка) |
| **Этап** | 1: Проектирование требований → 2: Архитектура |
| **Автор** | Мусхажиев Игорь Александрович |
| **Дата создания** | 05.05.2026 |
| **Версия документа** | 1.0 |

---

## 1. Назначение таблицы
Таблица трассировки обеспечивает сквозную прослеживаемость от бизнес-потребностей (Этап 0) до системных функций, REST-эндпоинтов и архитектурных слоёв. Используется для верификации полноты реализации, планирования тестов и аудита соответствия ТЗ.

---

## 2. Матрица трассировки

| BR-ID | Бизнес-требование | Use Case | REST API эндпоинт | Экран Android | Слой PCMEF (Клиент/Сервер) | Статус |
|:---|:---|:---|:---|:---|:---|:---|
| **BR-01** | Создание профессионального портфолио | UC2, UC7 | `POST /api/portfolios`<br>`PUT /api/portfolios/{id}` | `PortfolioCreateScreen`<br>`PortfolioSettingsScreen` | C → M → E → F | ✅ |
| **BR-02** | Загрузка и категоризация работ | UC3, UC4 | `POST /api/photos`<br>`PUT /api/albums/{id}` | `PhotoUploadScreen`<br>`AlbumEditScreen` | C → M → E → F | ✅ |
| **BR-03** | Оффлайн-работа и фоновая синхронизация | UC6 | `POST /api/sync/push`<br>`GET /api/sync/pull?since=` | `SyncIndicator` (глобальный)<br>`SettingsScreen` | F (Room + WorkManager) | ✅ |
| **BR-04** | Безопасная аутентификация и сессии | UC1 | `POST /api/auth/login`<br>`POST /api/auth/refresh` | `AuthScreen`<br>`BiometricPrompt` | C → M → F (JWT/BCrypt) | ✅ |
| **BR-05** | Демонстрация портфолио клиентам | UC8 | `GET /api/portfolios/{id}`<br>`GET /api/photos?albumId=` | `PortfolioViewerScreen`<br>`GalleryScreen` | P → C → M → F | ✅ |
| **BR-06** | Управление метаданными и тегами | UC4, UC5 | `PUT /api/photos/{id}/metadata`<br>`GET /api/photos/search?tag=` | `PhotoDetailsScreen`<br>`TagManagerScreen` | C → M → E → F | ✅ |
| **BR-07** | Обратная связь и аналитика просмотров | UC9 | `POST /api/comments`<br>`GET /api/analytics/views` | `CommentDialog`<br>`StatsScreen` (бонус) | C → M → E → F | 🟡 |
| **BR-08** | Управление ролями и доступом | UC5 | `GET /api/users/roles`<br>`PATCH /api/users/{id}/role` | `AdminPanelScreen` (бонус) | C → M → E → F |  |

**Легенда статусов:**
- ✅ **Реализовано/Запланировано в MVP**
-  **Отложено в бэклог (бонусные функции)**

---

## 3. Правила трассировки
1. Каждое бизнес-требование (BR) имеет минимум один связанный Use Case.
2. Каждый Use Case покрывается минимум одним REST-эндпоинтом (согласно п. 3.3.2 МУ: 8+ эндпоинтов).
3. Все эндпоинты маршрутизируются через слои PCMEF без циклических зависимостей.
4. Трассировка используется при написании тест-кейсов (п. 2.6.1 МУ: покрытие >40%).
