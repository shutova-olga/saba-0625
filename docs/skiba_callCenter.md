@startuml 
title Компонентная диаграмма системы управления очередью Call-центра

' --- Определение компонентов ---
component "Телефон (звонок)" as Phone
component "Компонент маршрутизации звонков (ACD)" as ACD
component "Компонент управления очередью" as Queue
component "Компонент управления приоритезацией" as Priority
component "Компонент распределения по операторам" as Distribution
component "Оператор" as Operator
component "Компонент записи разговоров" as Rec
component "Компонент сбора статистики и аналитики" as Stats
component "CRM" as CRM
component "Компонент аналитики" as Analytics
database "База данных операторов" as DB_Operators
database "База данных клиентов" as DB_Clients
database "Хранилище записей разговоров" as DB_Rec
database "Хранилище статистики" as DB_Stats

' --- Связи с внешними системами ---
Phone --> ACD : входящий звонок
ACD --> Priority : определить приоритет
ACD --> Distribution : распределить по операторам
Priority --> Queue : поставить звонок в очередь
Distribution --> Queue : поставить звонок в очередь
Queue --> Operator : соединить с оператором
CRM --> Operator : Предоставить данные о клиенте
Operator --> Rec : запись разговора
Operator --> Stats : сбор статистики и аналитики
Rec --> DB_Rec : запись разговора в базу
Stats --> DB_Stats : запись статистики в базу
CRM --> DB_Clients : база данных клиента
CRM --> DB_Operators : база данных оператора
Stats --> Analytics : передача данных для аналитики

