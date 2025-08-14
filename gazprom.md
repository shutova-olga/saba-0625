Преза по газовому промыслу

```
@startuml
title Система регистрации инцидентов на газовом промысле

' Определяем компоненты
component "Веб-приложение (Frontend)" as FE
component "Backend API" as API
component "PostgreSQL" as DB
component "Хранилище файлов" as FS
component "Kafka" as Kafka
component "HR-система" as HR
component "Система учёта оборудования" as EQ
component "BI-система" as BI

' Связи
FE --> API : REST API
API --> DB : SQL
API --> FS : File upload/download
API --> Kafka : Публикация событий
Kafka --> HR : События / интеграция
Kafka --> EQ : События / интеграция
Kafka --> BI : События / интеграция

@enduml
```

