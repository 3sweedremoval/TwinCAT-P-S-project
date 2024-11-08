# Clamping

```plantuml
@startuml
[*] --> Idle

Idle : clamping = FALSE
Idle --> Start_Clamping : gvl.startClamp = TRUE

Start_Clamping : clamping = TRUE
Start_Clamping : timerstart = TRUE
Start_Clamping --> Clamping : timerstart = TRUE

Clamping : Timer active, clamping in progress
Clamping --> Clamped : timerreached = TRUE (3Sec)

Clamped : clampingDone = TRUE
Clamped --> Release_Clamp : gvl.sawingProcessDone = TRUE

Release_Clamp : clampingDone = FALSE
Release_Clamp --> Idle : Clamp released, returning to idle
@enduml
