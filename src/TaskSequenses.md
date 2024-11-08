# Task sequenses

```plantuml

@startuml
actor User
participant UserInterface as UI
participant SawLengthStop as SLS
participant Clamp
participant Saw

User -> UI : Input sawing length and speed
UI -> SLS : startButtonLengthStop = TRUE
SLS -> UI : Position reached? (stopPosReached, metalIsToStop)
UI -> User : Message: "Move metal to stop" if metal not at stop
User -> UI : Moves metal to stop position

UI -> Clamp : startClamp = TRUE
Clamp -> UI : Clamping done (clamped)

UI -> Saw : Start sawing process (sawingSpeed)
Saw -> UI : Sawing process done (sawingProcessDone)

UI -> User : Message: "Sawing complete"
@enduml
