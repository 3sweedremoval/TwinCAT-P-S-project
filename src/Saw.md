# Saw
```plantuml
@startuml
[*] --> Idle

Idle : state = 0
Idle : startSawing = FALSE
Idle : sawingDone = FALSE
Idle --> Start_Sawing : startSawing = TRUE and sawingDone = FALSE

Start_Sawing : state = 1
Start_Sawing : actualPos = 100, timer1 active
Start_Sawing --> Saw_Down : timer1.q = TRUE

Saw_Down : state = 2
Saw_Down : Moving to bottomPos
Saw_Down --> Pause_at_Bottom : actualPos <= bottomPos and output2 = TRUE

Pause_at_Bottom : state = 3
Pause_at_Bottom : timer active
Pause_at_Bottom --> Saw_Up : timer.Q = TRUE

Saw_Up : state = 4
Saw_Up : Moving to topPos
Saw_Up --> Pause_at_Top : actualPos >= topPos

Pause_at_Top : state = 5
Pause_at_Top : timer active, input2 = TRUE, time2 = T#2S
Pause_at_Top --> Reset_and_Restart : output2 = TRUE

Reset_and_Restart : state = 6
Reset_and_Restart : input2 = TRUE, time2 = T#2S
Reset_and_Restart --> Idle : output2 = TRUE and reset sawingDone
@enduml
