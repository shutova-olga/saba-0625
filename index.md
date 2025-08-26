# Проверка рендера PlantUML

## Диаграмма 1
```plantuml
@startuml
Alice -> Bob: Hello
Bob --> Alice: Hi
@enduml
```

## Диаграмма 2
```plantuml
@startuml
actor User
User -> System: Запрос
System --> User: Ответ
@enduml
```

## Диаграмма 3
```plantuml
@startuml
class Car {
  +String model
  +drive()
}
class Engine
Car --> Engine
@enduml
```
