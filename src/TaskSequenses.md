# Task sequenses

```plantuml

@startuml
actor User
participant UserInterface as UI
participant SawLengthStop as SLS
participant Clamp
participant Saw

User -> UI : Input `inputSawingLength` and `inputSawingSpeed`
activate UI
UI -> UI : Set `gvl.requiredSawingLength = inputSawingLength`
UI -> UI : Set `gvl.sawingSpeed = inputSawingSpeed`

User -> UI : Press `startButtonLengthStop`
UI -> SLS : Set `gvl.startButtonLengthStop = TRUE`

SLS -> SLS : Move to position `gvl.requiredSawingLength`
SLS --> UI : Set `gvl.stopPosReached = TRUE`
UI -> UI : Check `gvl.metalIsToStop`

alt Metal not at stop
    UI -> User : Display message "Move metal to stop"
    User -> UI : Confirm metal at stop
    SLS -> UI : Set `gvl.metalIsToStop = TRUE`
end

UI -> Clamp : Set `gvl.startClamp = TRUE`
activate Clamp
Clamp -> UI : Set `gvl.clamped = TRUE`
deactivate Clamp

UI -> Saw : Start sawing with `gvl.sawingSpeed`
activate Saw
Saw -> Saw : Move from `topPos` to `bottomPos`
Saw -> UI : Set `gvl.bottomPosReached = TRUE`

Saw -> Saw : Move back to `topPos`
Saw -> UI : Set `gvl.sawingProcessDone = TRUE`
deactivate Saw

UI -> Clamp : Set `gvl.startClamp = FALSE` (declamp)
activate Clamp
Clamp -> UI : Set `gvl.clamped = FALSE`
deactivate Clamp

UI -> User : Display "Sawing complete"
UI -> UI : Start `timer1` to delay reset of `gvl.sawingProcessDone`
UI --> UI : After delay, reset `gvl.sawingProcessDone`
deactivate UI
@enduml
