# User interface

```plantuml

@startuml

[*] --> Idle

Idle : Waiting for user input
Idle --> Start_Length_Stop : startButtonLengthStop = TRUE
Idle --> Metal_Stop_Prompt : gvl.stopPosReached = TRUE and not gvl.metalIsToStop
Idle --> Start_Clamping : startClamp = TRUE and gvl.metalIsToStop = TRUE

Start_Length_Stop : Activating length stop positioning
Start_Length_Stop --> Idle : Length stop at pos

Metal_Stop_Prompt : Message: "Move metal to stop"
Metal_Stop_Prompt --> Idle : metalIsToStop = TRUE

Start_Clamping : Starting clamping process
Start_Clamping --> Idle : Clamping initiated

Idle --> Sawing_Done : gvl.sawingProcessDone = TRUE

Sawing_Done : Sawing process complete
Sawing_Done : timer1Start = TRUE, time1 = 3s
Sawing_Done --> Reset_Sawing_Done : timer1Reached = TRUE

Reset_Sawing_Done : Clearing sawingDone
Reset_Sawing_Done --> Idle : timer1 completes
@enduml
