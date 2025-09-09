@startuml

skinparam linetype ortho

title LMS - Component Diagram (Nested Packages)

 

actor Студент as s

actor Учитель as t

 

' --- Внешние системы ---

database Database as bd

  interface IUserStorage

  bd - IUserStorage

  interface IStorageService

  bd - IStorageService

 

note right of bd

  Бд где хранятся

  - пользователи

  - файлы

end note

 

node "Платежный Сервис" as pay

  interface IPayService

  pay - IPayService

 

note right of pay

  тут информация об оплате курса

    - не просрочены ли ежемесячные платежи

    - внесена ли полностью оплата

end note

 

' --- LMS System ---

package "LMS System" {

  package "Core Services" {

    component "User Management" as UserMgmt

      interface IAuth

      UserMgmt - IAuth

      interface IRoleManagement

      UserMgmt - IRoleManagement

 

      ' Требует

      UserMgmt ..( IUserStorage

      UserMgmt ..( IPayService

 

    note right of UserMgmt

      - аутентификация

      - роли

      - доступы

    end note

 

    component "Content Management" as Content

      interface IContentDelivery

      Content - IContentDelivery

      interface IContentUpload

      Content - IContentUpload

 

      ' Требует

      Content ..( IStorageService

      Content ..( IRoleManagement

 

    note right of Content

      - загрузка материалов

      - показ материалов

      - разграничение доступов

    end note

  }

 

  package "Learning Services" {

    component "Testing & Assessment" as Testing

      interface ITestService

      Testing - ITestService

 

      ' Требует

      Testing ..( IContentDelivery

      Testing ..( IAuth

      Testing ..( IStorageService

 

    note right of Testing

      - проведение тестов

      - загрузка материалов из content Management

      - аналитика собирает результаты

    end note

 

    component "Communication" as Comm

      interface IForumService

      Comm - IForumService

      interface IMessaging

      Comm - IMessaging

 

      ' Требует

      Comm ..( IAuth

 

    note right of Comm

      - форум

      - чаты

    end note

 

    component "Analytics" as Analytics

      interface IReporting

      Analytics - IReporting

      interface IProgressTracking

      Analytics - IProgressTracking

 

      ' Требует

      Analytics ..( ITestService

      Analytics ..( IContentDelivery

      Analytics ..( IAuth

 

    note right of Analytics

      - прогресс для студентов

      - отчет для учителей

    end note

  }

}

 

' --- UI Layer ---

package "UI" {

  component "Forum UI" as Forum

  Forum ..( IForumService )

 

  component "Chats UI" as Chats

  Chats ..( IMessaging )

 

  component "Testing UI" as TestUI

  TestUI ..( ITestService )

 

  component "Учебник UI" as BookUI

  BookUI ..( IContentDelivery )

 

  component "Отчеты UI" as ReportUI

  ReportUI ..( IReporting )

  ReportUI ..( IProgressTracking )

}

 

note right of UI

  Это веб интерфейс

end note

 

' --- Акторы и доступ ---

s --> IAuth : "логин"

t --> IAuth : "логин"

 

s --> Forum

t --> Forum

 

s --> TestUI

t --> TestUI

 

s --> BookUI

t --> BookUI

 

s --> ReportUI

t --> ReportUI

 

@enduml