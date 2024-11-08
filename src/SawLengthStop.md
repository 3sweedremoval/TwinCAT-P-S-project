# Saw length stop

```plantuml

@startuml
[*] --> Idle

Idle : Waiting for startButtonLengthStop signal
Idle --> Check_Position : gvl.startButtonLengthStop = TRUE

Check_Position : Comparing requiredSawingLength and sawingStopPos
Check_Position --> Position_Reached : sawingStopPos = requiredSawingLength
Check_Position --> Move_Stop : sawingStopPos <> requiredSawingLength

Move_Stop : Moving stop to requiredSawingLength
Move_Stop --> Position_Reached : sawingStopPos = requiredSawingLength

Position_Reached : posReached = TRUE
Position_Reached --> Metal_Against_Stop

Metal_Against_Stop : Checking metalAgainstStop
Metal_Against_Stop : gvl.metalIsToStop = metalAgainstStop
Metal_Against_Stop --> Idle : Resetting for next operation
@enduml
