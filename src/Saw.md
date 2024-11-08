# Saw
```plantuml
@startuml
[*] --> Idle

Idle : state = 0
Idle : startSawing = FALSE
Idle : sawingDone = FALSE
Idle --> Initialize_Sawing : startSawing = TRUE 

Initialize_Sawing : state = 1
Initialize_Sawing : actualPos = topPos, timer1 active (2s)
Initialize_Sawing --> Move_to_Bottom : timer1reached = TRUE

Move_to_Bottom : state = 2
Move_to_Bottom : Moving to bottomPos, timer1 active (1s)
Move_to_Bottom --> Pause_at_Bottom : actualPos <= bottomPos and timer1reached = TRUE

Pause_at_Bottom : state = 3
Pause_at_Bottom : timer1 active (2s)
Pause_at_Bottom --> Move_to_Top : timer1reached = TRUE

Move_to_Top : state = 4
Move_to_Top : Moving to topPos
Move_to_Top --> Pause_at_Top : actualPos >= topPos

Pause_at_Top : state = 5
Pause_at_Top : timer1 active (2s)
Pause_at_Top --> Reset_and_Restart : timer1reached = TRUE

Reset_and_Restart : state = 6
Reset_and_Restart : timer1 active (2s)
Reset_and_Restart --> Idle : timer1reached = TRUE and reset sawingDone
@enduml
