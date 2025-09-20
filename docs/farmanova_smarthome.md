@startuml
autonumber

actor "Пользователь" as User
participant "Мобильное приложение" as MobileApp
participant "Центральный хаб" as Hub
participant "Модуль связи Zigbee" as Zigbee
participant "Обработка данных сенсоров" as SensorProcessor
participant "Модуль правил/сценариев" as RulesEngine
participant "Управление устройствами" as DeviceController
participant "Сервис уведомлений" as NotificationService

== Управление пользователем ==
User -> MobileApp : Команда управления (например, включить свет)
MobileApp -> Hub : Передача команды
Hub -> DeviceController : Выполнение команды
DeviceController -> Light : Управление устройством
Hub -> NotificationService : Подтверждение выполнения
NotificationService --> MobileApp : Отображение статуса

== Выполнение сценариев ==
SensorProcessor -> RulesEngine : Передача обработанных данных
alt Условие сценария выполнено
    RulesEngine -> DeviceController : Команда на управление устройством
    DeviceController -> Light : Включение света
    DeviceController -> Thermostat : Регулировка температуры
    DeviceController -> Plug : Отключение розетки
    RulesEngine -> NotificationService : Запрос уведомления
    NotificationService --> MobileApp : Уведомление пользователя
else Условие сценария не выполнено
    RulesEngine --> SensorProcessor : Нет действий
end

== Сбор данных с датчиков ==
TempSensor -> Zigbee : Передача температуры
MotionSensor -> Zigbee : Событие движения
LeakSensor -> Zigbee : Сигнал протечки
Zigbee -> Hub : Передача данных
Hub -> SensorProcessor : Обработка данных сенсоров
@enduml
