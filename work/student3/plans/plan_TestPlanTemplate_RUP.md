BookSphere Technologies (Допущение)

# BookSphere Test Plan

Version 1.0

Date: 20/10/2026

Document identifier: BS-TP-001

## Revision History

| Date | Version | Description | Author |
|---|---|---|---|
| 20/10/2026 | 1.0 | Первоначальная версия тест-плана | QA Lead (Допущение) |

## Оглавление

1. Introduction  
1.1 Purpose  
1.2 Background  
1.3 Scope  
1.4 Project Identification  
2. Requirements for Test  
3. Test Strategy  
3.1 Testing Types  
3.1.1 Data and Database Integrity Testing  
3.1.2 Function Testing  
3.1.3 Business Cycle Testing  
3.1.4 User Interface Testing  
3.1.5 Performance Profiling  
3.1.6 Load Testing  
3.1.7 Stress Testing  
3.1.8 Volume Testing  
3.1.9 Security and Access Control Testing  
3.1.10 Failover and Recovery Testing  
3.1.11 Configuration Testing  
3.1.12 Installation Testing  
3.2 Tools  
4. Resources  
4.1 Roles  
4.2 System  
5. Project Milestones  
6. Deliverables  
6.1 Test Model  
6.2 Test Logs  
6.3 Defect Reports  
Appendix A Project Tasks  

# 1. Introduction

## 1.1 Purpose

Настоящий Test Plan для BookSphere определяет подход к проверке e-commerce платформы BookSphere, включая Web-витрину на Next.js, мобильное приложение Flutter для iOS и Android, каналы Smart TV и голосовых ассистентов, микросервисный backend в Kubernetes, административную панель, внешние интеграции и нефункциональные требования. Документ фиксирует тестовые требования, стратегию по 12 типам тестирования, ресурсы, этапы, трудозатраты и deliverables.

## 1.2 Background

BookSphere — масштабируемая e-commerce платформа для продажи печатных книг, электронных книг и аудиокниг. Платформа построена по архитектуре Headless Commerce и поддерживает каналы Web, Mobile App, Smart TV и голосовые ассистенты.

Клиентская часть включает умный каталог и поиск на Elasticsearch, карточку товара с форматами покупки, Look Inside с защитой от копирования, отзывы с модерацией, личный кабинет, «Мою библиотеку» с синхронизацией прогресса, систему лояльности, вишлисты и AI-рекомендации. Checkout поддерживает гостевой сценарий, расчёт доставки через API служб доставки, промокоды, сертификаты, бонусы, оплату картами, СБП, крипто-кошельками и рассрочкой.

Backend реализован как микросервисная архитектура в контейнерах и Kubernetes. Используются API Gateway Kong или AWS API Gateway, микросервисы на Python FastAPI с Elasticsearch, Node.js NestJS, Go Gin с Keycloak, Python PyTorch/TensorFlow, Headless CMS Strapi или Sanity, PostgreSQL, Redis, MongoDB, S3, DRM-сервер, Yandex Cloud или AWS, GitLab CI или GitHub Actions, Prometheus, Grafana, ELK Stack и Sentry.

Бизнес-модели включают прямые продажи, подписку, маркетплейс и B2B-лицензии. USP продукта: книжные клубы и социальные функции, AR-витрина, эко-трекинг углеродного следа, гибридный ридер с Immersion Reading. Внешние интеграции включают платёжные шлюзы ЮKassa, CloudPayments, Stripe, логистику, 1С:Предприятие, МойСклад, Bitrix24 или amoCRM, SendGrid, Twilio или SMS.ru, Telegram Bot API и API поставщиков контента.

## 1.3 Scope

В рамках плана рассматриваются уровни интеграционного, системного и приёмочного тестирования. Unit-тестирование выполняется командой разработки и проверяется через CI/CD, но не является основным предметом настоящего плана. План охватывает 12 типов тестирования из раздела 3.1.

Будут тестироваться: каталог и поиск, карточка товара, личный кабинет, AI-рекомендации, checkout, административная панель PIM, OMS, маркетинг и контент, BI, подписка, маркетплейс, B2B-лицензии, книжные клубы, AR-витрина, эко-трекинг, Immersion Reading, крипто-оплата, Smart TV, голосовые ассистенты, мобильное приложение Flutter, Web Next.js, внешние интеграции, безопасность, производительность, доступность и масштабируемость.

Не входят в основной scope: физические складские процессы и складское оборудование, кроме проверки интеграций OMS с 1С:Предприятие и МойСклад; юридическая сертификация PCI DSS, при этом проверяется соответствие техническим требованиям PCI DSS; производственная эксплуатация внешних платёжных и логистических провайдеров, которая проверяется через тестовые среды и заглушки.

Допущения: тестовые среды максимально приближены к production в Yandex Cloud или AWS и Kubernetes (Допущение); внешние интеграции имеют тестовые контуры или заглушки (Допущение); тестовые данные маскируются и не содержат реальных персональных данных (Допущение).

Риски и меры снижения (Допущение для оценок):

| Риск | Мера снижения |
|---|---|
| Нестабильность внешних API платёжных шлюзов, логистики, 1С и CRM | Ранние контрактные тесты, заглушки, мониторинг доступности, тестовые контуры провайдеров |
| Сложность интеграции микросервисов через API Gateway и Keycloak | Интеграционные тесты по контрактам, проверка сквозных сценариев, логирование в ELK и Sentry |
| Несоответствие производительности требованиям API < 200 мс и 10 000 одновременных пользователей | Раннее профилирование, нагрузочные сценарии k6 или JMeter (Допущение), автоскейлинг Kubernetes, контроль p95 |
| Дефекты DRM и синхронизации Immersion Reading между устройствами | Отдельные тесты DRM, синхронизации прогресса, аудиоплеера Flutter и мобильных оффлайн-сценариев |
| Ограничения Smart TV и голосовых ассистентов | Матрица устройств и платформ, ранние smoke-тесты, проверка голосовых сценариев и remote control |
| Риски AR-витрины на камере смартфона | Тесты на поддерживаемых устройствах, проверка разрешений камеры, деградация при отсутствии AR |
| Нагрузка на PostgreSQL, Redis, MongoDB, Elasticsearch и S3 | Профилирование, volume-тесты, контроль лимитов, резервные стратегии и регламенты очистки |
| Риски безопасности PCI DSS, 152-ФЗ, GDPR, DDoS и SQL-инъекций | Security-тесты, сканирование уязвимостей, проверка Keycloak, аудит доступа, координация с ИБ |

Ограничения: финальные сроки зависят от готовности внешних тестовых контуров; нагрузочное тестирование выполняется в отдельном окне; для Smart TV и голосовых ассистентов доступность устройств ограничена матрицей, согласованной с продуктом (Допущение).

## 1.4 Project Identification

| Document | Created or Available | Received or Reviewed | Author or Resource | Notes |
|---|---|---|---|---|
| Requirements Specification | Да | Да | task.md | Паспорт продукта BookSphere используется как источник требований |
| Functional Specification | Нет | Нет | Не предоставлено | Отдельный документ не следует из task.md |
| Use-Case Reports | Нет | Нет | Не предоставлено | Отдельные отчёты не следуют из task.md |
| Project Plan | Нет | Нет | Не предоставлено | Есть этапы в разделе 9 task.md, отдельный план не предоставлен |
| Design Specifications | Нет | Нет | Не предоставлено | Отдельные спецификации дизайна не следуют из task.md |
| Prototype | Нет | Нет | Не предоставлено | Прототип не предоставлен |
| User’s Manuals | Нет | Нет | Не предоставлено | Руководства пользователя не следуют из task.md |
| Business Model or Flow | Да | Да | task.md | Раздел 7 task.md описывает бизнес-модели и монетизацию |
| Data Model or Flow | Нет | Нет | Не предоставлено | Модель данных не предоставлена |
| Business Functions and Rules | Да | Да | task.md | Разделы 3, 7 и 8 task.md описывают функции и USP |
| Project or Business Risk Assessment | Нет | Нет | Не предоставлено | Отдельная оценка рисков не предоставлена |

# 2. Requirements for Test

| ID | Requirement for Test | Source or Component |
|---|---|---|
| REQ-01 | Проверить умный каталог и поиск с учётом опечаток, синонимов и морфологии, фильтрацию по жанрам, авторам, издательствам, году, рейтингу, цене и формату | Elasticsearch, Web Next.js, Mobile Flutter, Smart TV, голосовые ассистенты |
| REQ-02 | Проверить карточку товара: печатный, электронный и аудиоформаты, Look Inside с защитой от копирования, видеообзоры, инфографику, похожие книги, отзывы с фото и видео и модерацией | Карточка товара, S3, DRM, MongoDB |
| REQ-03 | Проверить личный кабинет: историю заказов, статусы доставки в реальном времени, «Мою библиотеку», синхронизацию прогресса чтения и прослушивания между устройствами, лояльность, вишлисты и совместные вишлисты | Пользователи, Keycloak, PostgreSQL, Redis, Mobile Flutter, Web Next.js |
| REQ-04 | Проверить AI-рекомендации и персональную ленту «Для вас» на основе прочитанного, оценок и поведения | AI и рекомендации, PyTorch или TensorFlow |
| REQ-05 | Проверить оформление заказа: гостевой checkout, расчёт доставки через API служб доставки, промокоды, сертификаты, бонусы, оплату картами, СБП, крипто-кошельками и рассрочкой | Checkout, API Gateway, платёжные шлюзы, логистика |
| REQ-06 | Проверить административную панель PIM: массовый импорт и экспорт CSV, XML, API, управление атрибутами, авторами и сериями | Back-office, PIM, PostgreSQL, S3 |
| REQ-07 | Проверить OMS: воркфлоу статусов, автоматическое распределение заказов по складам, печать накладных и штрихкодов | OMS, 1С:Предприятие, МойСклад |
| REQ-08 | Проверить маркетинг и контент: конструктор лендингов, баннеры, рассылки, блог и журнал о книгах | Headless CMS Strapi или Sanity |
| REQ-09 | Проверить аналитику и BI: дашборды LTV, CAC, конверсии, ABC-анализ ассортимента и прогнозирование спроса | BI, PostgreSQL, MongoDB |
| REQ-10 | Проверить подписку с безлимитным чтением и прослушиванием каталога за фиксированную плату в месяц | Subscription, платёжные шлюзы, DRM |
| REQ-11 | Проверить маркетплейс сторонних продавцов, комиссию, размещение товаров и взаимодействие с каталогом | Marketplace, PIM, OMS |
| REQ-12 | Проверить B2B-лицензии на корпоративные доступы к non-fiction литературе для компаний | B2B-лицензии, Keycloak, Subscription |
| REQ-13 | Проверить книжные клубы, голосовые рецензии, публичные полки с друзьями | Социальные функции, MongoDB, S3 |
| REQ-14 | Проверить AR-витрину: наведение камеры смартфона на физическую книгу, трейлер, отзывы и заказ доставки | AR-витрина, Mobile Flutter, камера смартфона |
| REQ-15 | Проверить эко-трекинг углеродного следа доставки и компенсацию посадкой виртуального и реального дерева | Эко-трекинг, checkout, логистика |
| REQ-16 | Проверить Immersion Reading: синхронную подсветку текста с голосом чтеца в аудиоплеере | Гибридный ридер, Mobile Flutter, DRM |
| REQ-17 | Проверить крипто-оплату через платёжные шлюзы и корректность статусов заказа | Checkout, платёжные шлюзы |
| REQ-18 | Проверить канал Smart TV: каталог, карточку, воспроизведение и покупку в рамках поддерживаемых сценариев | Smart TV |
| REQ-19 | Проверить голосовые ассистенты: поиск, информацию о книге, заказ и уведомления в рамках поддерживаемых сценариев | Голосовые ассистенты |
| REQ-20 | Проверить мобильное приложение Flutter для iOS и Android: нативный аудиоплеер, оффлайн-режим для e-books, синхронизацию | Mobile Flutter |
| REQ-21 | Проверить Web Next.js с SSR и SSG: SEO, скорость загрузки, навигацию и адаптивность | Web Next.js |
| REQ-22 | Проверить внешние интеграции: платёжные шлюзы, логистику, 1С, МойСклад, CRM, SendGrid, Twilio или SMS.ru, Telegram Bot API, API поставщиков контента | API Gateway, интеграционные адаптеры |
| REQ-23 | Проверить безопасность: PCI DSS, 152-ФЗ, GDPR, защиту от DDoS и SQL-инъекций, разграничение доступа | Keycloak, API Gateway, PostgreSQL, DRM |
| REQ-24 | Проверить производительность: API < 200 мс, до 10 000 одновременных пользователей, SLA 99.9%, автоскейлинг | Kubernetes, Prometheus, Grafana |
| REQ-25 | Проверить DRM-сервер: шифрование файлов и защиту электронных книг от нелегального копирования | DRM-сервер, S3, Mobile Flutter, Web Next.js |
| REQ-26 | Проверить роли внутренних пользователей: контент-менеджеров, маркетологов, операторов колл-центра и сотрудников склада | Back-office, Keycloak, OMS, PIM |

# 3. Test Strategy

Стратегия основана на риск-ориентированном подходе: в первую очередь проверяются сквозные сценарии покупки, подписки, оплаты, DRM, синхронизации, внешних интеграций и нагрузки. Для каждого типа тестирования определены цель, техника, критерии завершения и особенности.

## 3.1 Testing Types

### 3.1.1 Data and Database Integrity Testing

Test Objective: Убедиться, что методы и процессы доступа к данным BookSphere работают корректно и без повреждения данных в PostgreSQL, Redis, MongoDB, S3 и Elasticsearch.

Technique: Проверять доступ к данным напрямую и через API Gateway для заказов, пользователей, финансов, корзин, сессий, отзывов, логов, медиафайлов и поисковых индексов. Заполнять PostgreSQL, Redis, MongoDB и Elasticsearch валидными и невалидными данными, проверять ограничения, транзакции, TTL, целостность ссылок на объекты S3, переиндексацию Elasticsearch и метаданные DRM.

Completion Criteria: 100% запланированных проверок целостности выполнено; критических повреждений данных нет; открытых дефектов Critical и High — 0; расхождения между OMS, PostgreSQL и интеграцией с 1С или МойСклад отсутствуют или находятся в допустимом пороге 0,1% (Допущение).

Special Considerations: Использовать изолированные тестовые БД и маскированные данные; для прямого доступа применять DBeaver, pgAdmin, redis-cli и mongosh (Допущение); начинать с малых наборов данных для повышения наблюдаемости.

### 3.1.2 Function Testing

Test Objective: Проверить корректность функций BookSphere: навигацию, ввод данных, обработку, сохранение и извлечение данных для Web Next.js, Mobile Flutter, Smart TV, голосовых ассистентов и back-office.

Technique: Выполнять use cases и бизнес-правила через GUI и API для каталога, карточки товара, личного кабинета, checkout, подписки, маркетплейса, B2B, книжных клубов, AR-витрины, эко-трекинга, Immersion Reading, крипто-оплаты и внешних интеграций. Проверять валидные и невалидные данные, сообщения об ошибках, роли Keycloak и ответы API Gateway Kong или AWS API Gateway.

Completion Criteria: 100% запланированных функциональных тест-кейсов выполнено; не менее 95% успешных прохождений (Допущение); открытых дефектов Critical и High — 0; Medium — не более 5 (Допущение).

Special Considerations: Учитывать различия каналов Web, Mobile, Smart TV и голосовых ассистентов; внешние провайдеры тестировать через тестовые контуры или заглушки; проверять бизнес-правила подписки, маркетплейса и B2B отдельно.

### 3.1.3 Business Cycle Testing

Test Objective: Убедиться, что бизнес-процессы и фоновые процессы BookSphere работают по требуемым бизнес-моделям и расписаниям.

Technique: Смоделировать бизнес-циклы: ежемесячное продление подписки, выплаты и комиссии маркетплейса, выдачу B2B-лицензий, события книжных клубов, начисление и списание бонусов, компенсацию углеродного следа, заказ-доставку, промоакции и подтверждение крипто-оплаты. Использовать функции checkout, subscription, marketplace, B2B, эко-трекинга и OMS с валидными и невалидными датами.

Completion Criteria: 100% запланированных бизнес-циклов выполнено; все периодические задания запущены по расписанию; расхождения финансовых и статусных данных — 0; открытых дефектов Critical и High — 0.

Special Considerations: Требуется управление системными датами; бизнес-модели из task.md используются как основа сценариев; внешние платёжные и логистические статусы синхронизируются через тестовые контуры.

### 3.1.4 User Interface Testing

Test Objective: Проверить, что интерфейсы Web Next.js, Mobile Flutter, Smart TV и голосовых ассистентов обеспечивают корректную навигацию, доступ к функциям и соответствие стандартам.

Technique: Проверять экраны и переходы, работу Tab, мыши, клавиатуры, пульта Smart TV, жестов Mobile Flutter и голосовых команд. Проверять адаптивность, состояния объектов, фокус, меню, ошибки валидации и доступность основных сценариев в каталоге, карточке, checkout, личном кабинете и back-office.

Completion Criteria: 100% экранов и голосовых сценариев из матрицы проверено; не менее 95% UI-проверок пройдено (Допущение); открытых дефектов Critical и High — 0.

Special Considerations: Матрица устройств и браузеров согласуется с продуктом (Допущение); кастомные компоненты и сторонние виджеты могут иметь ограничения доступа к свойствам; для Smart TV и голосовых ассистентов проверять деградацию сценариев.

### 3.1.5 Performance Profiling

Test Objective: Проверить время ответа, транзакционную производительность и достижение требований API < 200 мс, SLA 99.9% и поддержки 10 000 одновременных пользователей.

Technique: Использовать сценарии Function и Business Cycle Testing для API Gateway, каталога Elasticsearch, корзины, заказов, checkout, оплаты, DRM, AI-рекомендаций и личного кабинета. Сначала измерять одного пользователя и одну транзакцию, затем повторять с несколькими клиентами; собирать метрики через Prometheus, Grafana, ELK Stack и Sentry.

Completion Criteria: Для одной транзакции — успешное выполнение без ошибок; для нескольких пользователей — API < 200 мс на p95 (Допущение для p95), ошибки менее 1% (Допущение), SLA 99.9% подтверждён на тестовом окне.

Special Considerations: Выполнять на выделенном окружении, приближенном к production; использовать БД фактического или масштабированного размера; учитывать фоновую нагрузку и автоскейлинг Kubernetes.

### 3.1.6 Load Testing

Test Objective: Проверить поведение BookSphere при различных рабочих нагрузках вплоть до 10 000 одновременных пользователей.

Technique: Использовать сценарии поиска, просмотра карточки, корзины, checkout, подписки, оплаты и личного кабинета. Генерировать нагрузку через k6 или JMeter (Допущение), проверять API Gateway, микросервисы, PostgreSQL, Redis, MongoDB, Elasticsearch и S3, контролировать автоскейлинг Kubernetes.

Completion Criteria: При 10 000 одновременных пользователей API < 200 мс на p95 (Допущение), ошибки менее 1% (Допущение), отсутствие деградации SLA 99.9%; все запланированные нагрузочные сценарии выполнены.

Special Considerations: Проводить на выделенном окружении и в отдельном временном окне; использовать production-like данные; внешние сервисы заменять заглушками или тестовыми контурами.

### 3.1.7 Stress Testing

Test Objective: Найти ошибки BookSphere при нехватке ресурсов, конкуренции за ресурсы и определить предельную нагрузку.

Technique: Использовать нагрузочные сценарии и ограничивать ресурсы pod в Kubernetes, память Redis, соединения PostgreSQL, heap Elasticsearch и пропускную способность сети. Проверять одновременную работу пользователей с одними данными, худший транзакционный микс, конкурентное оформление заказов и оплату.

Completion Criteria: Все запланированные стресс-тесты выполнены; система достигает или превышает заданные лимиты без дефектов вне допустимых условий; при отказе фиксируются условия, восстановление и отсутствие повреждения данных.

Special Considerations: Стресс-тесты интрузивны; выполнять в изолированном namespace Kubernetes, после часов или на отдельном стенде; синхронизировать доступ к одним записям и данным.

### 3.1.8 Volume Testing

Test Objective: Проверить работу BookSphere при больших объёмах данных и длительной нагрузке.

Technique: Создавать большие наборы книг, отзывов, заказов, логов, медиафайлов S3, индексов Elasticsearch и записей MongoDB. Проверять PIM-импорт CSV, XML и API, BI-отчёты, поиск, каталог и множественные одновременные запросы при максимальном размере БД.

Completion Criteria: 100% запланированных volume-тестов выполнено; система обрабатывает целевые объёмы без отказов; отчёты BI и PIM корректны; открытых дефектов Critical и High — 0.

Special Considerations: Определить acceptable time window для высоких объёмов (Допущение); данные генерировать автоматически; контролировать рост PostgreSQL, MongoDB, S3 и Elasticsearch.

### 3.1.9 Security and Access Control Testing

Test Objective: Проверить application-level и system-level безопасность: доступ к функциям и данным по ролям, защиту платёжных и персональных данных, устойчивость к DDoS и SQL-инъекциям.

Technique: Составить перечень ролей B2C, B2B, контент-менеджер, маркетолог, оператор колл-центра, сотрудник склада и администратор. Проверять Keycloak, API Gateway, back-office, checkout, платёжные шлюзы, DRM, S3 и PostgreSQL. Выполнять проверки PCI DSS, 152-ФЗ, GDPR, SQL-инъекций, DDoS-сценариев и разграничения доступа.

Completion Criteria: Для каждой роли доступны только разрешённые функции и данные; 100% security-тестов выполнено; критических уязвимостей — 0; High-уязвимости исправлены или имеют согласованный план.

Special Considerations: Координировать с ИБ и администраторами; использовать маскированные данные; не проводить разрушительные DDoS-тесты в production.

### 3.1.10 Failover and Recovery Testing

Test Objective: Убедиться, что BookSphere восстанавливается после отказов Kubernetes, PostgreSQL, Redis, S3, DRM и сети без недопустимой потери данных.

Technique: Использовать транзакции Function и Business Cycle Testing; имитировать завершение pod, drain узла Kubernetes, сетевой раздел, отказ PostgreSQL, Redis, S3 и DRM, прерывание циклов синхронизации и некорректные ключи или указатели в тестовой БД. Проверять восстановление и отчётность.

Completion Criteria: После восстановления система возвращается в известное корректное состояние; повреждение данных ограничено известными полями; RTO не более 1 часа и RPO не более 15 минут (Допущение); отчёты о незавершённых процессах формируются.

Special Considerations: Тесты интрузивны; выполнять в изолированном окружении, после часов, с участием DevOps, DBA и сетевой команды; использовать альтернативные методы имитации отказов.

### 3.1.11 Configuration Testing

Test Objective: Проверить работу BookSphere на разных аппаратных и программных конфигурациях.

Technique: Использовать функциональные сценарии на матрице браузеров Chrome, Firefox, Safari и Edge (Допущение), ОС, устройств iOS и Android для Flutter, моделей Smart TV, платформ голосовых ассистентов и сетевых условий 3G, 4G, 5G и Wi-Fi. Проверять конфигурации Kubernetes и внешних интеграций.

Completion Criteria: Для каждой комбинации матрицы все выбранные транзакции завершаются без отказов; 100% матрицы покрыто; открытых дефектов Critical и High — 0.

Special Considerations: Матрица устройств и браузеров должна быть согласована и документирована; учитывать ограничения сторонних приложений и платформ; фиксировать версии ОС и устройств.

### 3.1.12 Installation Testing

Test Objective: Проверить установку и обновление BookSphere для новых и ранее установленных версий Mobile Flutter, Web-развёртывания в Kubernetes и клиентов Smart TV и голосовых ассистентов.

Technique: Проверять новую установку мобильного приложения через TestFlight и Google Play Internal (Допущение), обновление с той же и более старой версии, развёртывание микросервисов в Kubernetes через CI/CD, обновление Helm-релизов и откат. После установки запускать подмножество функциональных smoke-тестов.

Completion Criteria: Установка и обновление успешны для всех целевых конфигураций; smoke-тесты проходят; откат успешен; открытых дефектов Critical и High — 0.

Special Considerations: Выбрать confidence-набор транзакций; учитывать подписи сертификатов, правила магазинов приложений и процедуры CI/CD; для Kubernetes проверять новые namespace и обновление версий микросервисов.

## 3.2 Tools

| | Tool | Vendor or In-house | Version |
|---|---|---|---|
| Test Management | TestRail (Допущение) | Vendor | Актуальная на 20/10/2026 (Допущение) |
| Defect Tracking | Jira (Допущение) | Vendor | Актуальная на 20/10/2026 (Допущение) |
| ASQ Tool for functional testing | Playwright, Appium, Flutter integration_test (Допущение) | Vendor и In-house | Актуальная на 20/10/2026 (Допущение) |
| ASQ Tool for performance testing | k6 или JMeter (Допущение) | Vendor | Актуальная на 20/10/2026 (Допущение) |
| Test Coverage Monitor or Profiler | SonarQube и JaCoCo (Допущение) | Vendor | Актуальная на 20/10/2026 (Допущение) |
| Project Management | Jira и Jira Advanced Roadmaps (Допущение) | Vendor | Актуальная на 20/10/2026 (Допущение) |
| DBMS tools | DBeaver, pgAdmin, redis-cli, mongosh (Допущение) | Vendor | Актуальная на 20/10/2026 (Допущение) |

# 4. Resources

## 4.1 Roles

| Worker | Minimum Resources Recommended | Specific Responsibilities or Comments |
|---|---|---|
| Test Manager, Test Project Manager | 1 FTE (Допущение) | Управление тестированием, техническое направление, ресурсы, отчётность |
| Test Designer | 3 FTE (Допущение) | Тест-план, тест-модель, тест-кейсы, оценка эффективности тестирования |
| Tester | 6 FTE (Допущение) | Выполнение тестов, логирование результатов, восстановление после ошибок, документирование запросов на изменение |
| Test System Administrator | 1 FTE (Допущение) | Администрирование TestRail и Jira, установка и доступ к тестовым системам, окружения Kubernetes |
| Database Administrator, Database Manager | 1 FTE (Допущение) | Администрирование тестовых PostgreSQL, Redis, MongoDB, Elasticsearch и данных S3 |
| Designer | 2 FTE (Допущение) | Определение тестовых классов, атрибутов и связей, тестовых пакетов |
| Implementer | 2 FTE (Допущение) | Реализация и unit-тестирование тестовых классов и пакетов, автоматизация Playwright, Appium, Flutter integration_test и k6 |

## 4.2 System

| Resource | Name or Type |
|---|---|
| Database Server Network or Subnet | test-vpc, 10.10.0.0/24 (Допущение) |
| Database Server Server Name | booksphere-test-pg (Допущение) |
| Database Server Database Name | booksphere_test (Допущение) |
| Client Test PCs Include special configuration requirements | Windows и macOS, Chrome, Firefox, Safari, Edge; устройства iOS и Android для Flutter; Smart TV; платформы голосовых ассистентов (Допущение) |
| Test Repository Network or Subnet | test-vpc, 10.10.1.0/24 (Допущение) |
| Test Repository Server Name | booksphere-test-repo (Допущение) |
| Test Development PCs | booksphere-test-dev (Допущение) |

# 5. Project Milestones

| Milestone Task | Effort | Start Date | End Date |
|---|---|---|---|
| Plan Test | 8 человеко-недель (Допущение) | 01/01/2027 | 28/02/2027 |
| Design Test | 24 человеко-недели (Допущение) | 01/01/2027 | 31/05/2027 |
| Implement Test | 32 человеко-недели (Допущение) | 01/03/2027 | 30/06/2027 |
| Execute Test | 96 человеко-недель (Допущение) | 01/03/2027 | 31/10/2027 |
| Evaluate Test | 16 человеко-недель (Допущение) | 01/11/2027 | 30/11/2027 |

Этапы согласованы с разделом 9 task.md: Discovery 1–2 месяца, Проектирование и Дизайн 2 месяца, Разработка MVP 3–4 месяца, Разработка Full-версии 3–4 месяца, QA и нагрузочное тестирование 1 месяц, Релиз и Поддержка. Даты рассчитаны от старта проекта 01/11/2026 (Допущение). Планирование и дизайн тестов ведутся с этапа проектирования, тестирование MVP и Full-версии — на своих этапах, финальное QA и нагрузочное тестирование — 1 месяц.

# 6. Deliverables

## 6.1 Test Model

| Deliverable | Author | Recipient | When |
|---|---|---|---|
| Test Plan BS-TP-001 | Test Manager | Project Stakeholders | 28/02/2027 |
| Матрица тестовых требований REQ-01–REQ-26 | Test Designer | QA Team и Product Owner | 28/02/2027 |
| Набор тест-кейсов по модулям и USP | Test Designer | QA Team | 31/05/2027 |
| Тестовые данные и заглушки внешних интеграций | Test System Administrator и DBA | QA Team | 30/06/2027 |
| Автотесты Playwright, Appium, Flutter integration_test и k6 | Implementer | QA Team | 30/06/2027 |
| Отчёт покрытия и результаты выполнения | Test Manager | Project Stakeholders | 30/11/2027 |

## 6.2 Test Logs

Логи выполнения ведутся в TestRail и Jira (Допущение). Ежедневные результаты фиксируются Testers, еженедельный сводный отчёт формирует Test Manager. Для диагностики используются Prometheus, Grafana, ELK Stack и Sentry.

## 6.3 Defect Reports

Дефекты регистрируются в Jira (Допущение) с severity, приоритетом, шагами воспроизведения, окружением, логами и привязкой к REQ. Отчёты о дефектах и статусах формируются еженедельно и на этапе Evaluate Test.

# Appendix A Project Tasks

## Plan Test

Plan Test — identify requirements for test — привязка: Jira и TestRail, REQ-01–REQ-26.  
Plan Test — assess risk — привязка: реестр рисков Jira, компоненты API Gateway, Keycloak, DRM, Kubernetes.  
Plan Test — develop test strategy — привязка: раздел 3 Test Plan, Web Next.js, Mobile Flutter, Smart TV, голосовые ассистенты.  
Plan Test — identify test resources — привязка: Yandex Cloud или AWS, Kubernetes test namespace, PostgreSQL, Redis, MongoDB, S3.  
Plan Test — create schedule — привязка: Jira Advanced Roadmaps, этапы раздела 9 task.md.  
Plan Test — generate Test Plan — привязка: документ BS-TP-001.

## Design Test

Design Test — prepare workload analysis — привязка: Prometheus, Grafana, сценарии k6 для checkout, поиска, подписки и DRM.  
Design Test — identify and describe test cases — привязка: TestRail, модули каталога, подписки, маркетплейса, B2B, книжных клубов, AR, эко-трекинга и Immersion Reading.  
Design Test — identify and structure test procedures — привязка: Playwright для Web Next.js, Appium и Flutter integration_test для Mobile Flutter, проверки Smart TV и голосовых ассистентов.  
Design Test — review and assess test coverage — привязка: TestRail, SonarQube, JaCoCo, матрица REQ-01–REQ-26.

## Implement Test

Implement Test — record or program test scripts — привязка: Playwright, Appium, Flutter integration_test, k6 или JMeter, Postman (Допущение).  
Implement Test — identify test-specific functionality in the Design and Implementation Model — привязка: API Gateway Kong или AWS API Gateway, Keycloak, платёжные шлюзы, DRM, OMS и PIM.  
Implement Test — establish external data sets — привязка: PostgreSQL, Redis, MongoDB, S3, Elasticsearch, тестовые данные 1С:Предприятие и МойСклад.

## Execute Test

Execute Test — execute Test procedures — привязка: TestRail, GitLab CI или GitHub Actions, Kubernetes test namespace.  
Execute Test — evaluate execution of Test — привязка: Prometheus, Grafana, Sentry, ELK Stack.  
Execute Test — recover from halted Test — привязка: Kubernetes rollback, CI/CD, резервные копии PostgreSQL и Redis.  
Execute Test — verify the results — привязка: сверка OMS, PostgreSQL, 1С:Предприятие и МойСклад, проверка DRM и S3.  
Execute Test — investigate unexpected results — привязка: Sentry, ELK Stack, Grafana, логи API Gateway.  
Execute Test — log defects — привязка: Jira, TestRail, привязка к REQ-01–REQ-26.

## Evaluate Test

Evaluate Test — evaluate Test-case coverage — привязка: TestRail и матрица REQ-01–REQ-26.  
Evaluate Test — evaluate code coverage — привязка: SonarQube и JaCoCo.  
Evaluate Test — analyze defects — привязка: Jira, severity и приоритеты дефектов.  
Evaluate Test — determine if Test Completion Criteria and Success Criteria have been achieved — привязка: Test Plan, метрики API < 200 мс, 10 000 пользователей и SLA 99.9%.