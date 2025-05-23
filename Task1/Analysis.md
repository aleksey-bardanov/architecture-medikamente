# Обеспечение безопасности данных Медикаменте

## Результаты аудита мер безопасности

- Сотрудники имеют доступ к данным, к которым они не должны иметь доступ (нарушается принцип минимальной осведомленности)
- Сотрудники имеют возможность менять данные, которые они не должны менять
- Данные не классифицированы и хранятся не системно
- Персональные денные клиентов не защищены шифрованием, маскировкой, деперсонализацией
- Не мониторится доступ к данным, вносимые изменения не отслеживаются
- Ни какие данные не защищены ни на каких этапах работы с ними (хранение, передача, работа)
- Данные не защищены от внесения ошибки человеком при обработке
- Система не масштабируется, при увеличении количества данных выбранные подходы не сработают

## Предложения по обеспечению безопасности данных

Так как компания планирует расширение, требуется менять подход к работе с данными в первую очередь из-за увеличения количества данных, которые передаются и хранятся в системе, во вторую очередь из-за растущего в связи с повышением числа внешних интеграций риска утечки данных.

### Классификация и тэгирование данных

Требуется для того, чтобы обеспечить безопасность интеграций с внешними системами и упростить разработку новых цифровых сервисов для клиентов компании. Также такой подход обеспечит удобство внедрения систем сбора и анализа данных.

#### Классы данных в системе 

- Открытые данные (tag: medikamente_public)
  - Публичные данные компании
  - Расписание специалистов

- Данные пациентов (tag: patients_protected)
  - ФИО 
  - Номер телефона 
  - Адрес электронной почты
  - Посещения

- Конфиденциальные данные пациентов (tag: patients_private)
  - Паспортные данные
  - Данные страхового полиса
  - СНИЛС
  - Адрес регистрации/проживания
  - Место работы/учебы
  - Информация о хронических заболеваниях

- Клинические данные (tag: medical_private)
  - Анамнезы
  - Результаты осмотров
  - Результаты анализов
  - Диагнозы
  - История болезни

- Коммерческие данные (tag: medikamente_private)
  - Движение денежных средств
  - Данные ТМЦ
  - Сформированные записи к специалистам
  - Счета на оплату

#### Рекомендуемые инструменты для мониторинга и аудит тегированных данных

Учитывая рост объема обрабатываемых данных и требования, предъявляемые к компании важно решить задачи автоматизации классификации и тегирования данных, обеспечить своевременное обнаружение возможных утечек данных, а также учесть требования законодательства по учету согласия пациентов на обработку персональных данных. Предлагаемые инструменты:

- Для отслеживания применения тегов и обнаружения возможных несоответствий рекомендуется использовать **Collibra Data Intelligence Cloud**. Эта платформа поможет выявить непротегированные данные и обеспечит отслеживаемость и эффективное управление ими.

- Для автоматической классификации данных и идентификации возможных утечек можно рассмотреть следующие системы:
  - **BigID**
  - **Informatica Axon**

Эти инструменты помогут автоматизировать процесс классификации данных и повысить уровень безопасности.

- Для выполнения требований законодательства в части контроля согласий пользователей и автоматизации управления данными подойдёт **OneTruth**.

- Для отслеживания путей данных в реальном времени рекомендуется использовать **DataGrail**.

- **Apache Atlas** можно рассмотреть как платформу управления метаданными если будут использоваться другие продукты из экосистемы Hadoop.

Выбор конкретной системы зависит от прочих принятых в проекте решений и от финансовых возможностей компании.

### Разделение доступа к данным по ролям, минимизация данных

Планируется расширение штата компании и открытие филиалов, чтобы обеспечить безопасность данных клиентов и соответствие требованиям законодательства требуется разделить доступ к данным по крайней мере в рамках ролевой модели RBAC.

#### Роли сотрудников и необходимые им данные в соответствие с принципом минимальной осведомленности

| Роль | Доступные данные | Разрешение на чтение(R)/запись(W) |
| - | - | - |
| Сотрудник ресепшена | Открытые данные<br> Данные пациентов | R<br> W |
| Кассир | Данные пациентов  | R |
| Бухгалтер | Открытые данные<br> Коммерческие данные<br> Данные пациентов | R<br> W<br> R |
| Сотрудник склада | Коммерческие данные (Движение ДС)<br> Коммерческие данные (Данные ТМЦ) | R<br> W |
| Специалист-медик | Данные пациентов<br> Клинические данные | R<br> W |
| Аналитик данных | Клинические данные<br> Коммерческие данные<br> Открытые данные | R<br> R<br> R |

### Обфускация, шифрование, маскирование данных

Для защиты персональных данных пациентов в системе предлагается:

- Токенизировать чувствительные данные при осуществлении платежных операций и операций с внешними сервисами
- Маскирование и перемешивание данных при передаче их аналитикам и в среды разработки
- Зануление чувствительных данных при открытии части данных сотрудникам без соответсвующей роли
- Шифрование чувствительных данных клиентов при хранении на серверах компании
- Шифрование данных при передаче по сети (TLS, SSH, VPN)

Для защиты данных компании предлагается:

- Использование шифрованных дисков или шифрованных папок на собственном сервере предприятия при использовании файловой 1С
- В случае перехода к базам 1С в СУБД формате через сервер 1С использовать защищенные сетевые соединения, шифрование БД
