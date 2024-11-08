# Function sequense


```plantuml

@startuml
actor "User Interface" as UI
actor "Saw Length Stop" as SLS
actor "Saw" as Saw
participant "POU_MoveToPos" as MoveToPos

== Saw Length Stop Task ==
UI -> SLS : Start stop positioning
SLS -> MoveToPos : Set `requiredPos = gvl.requiredSawingLength`, `actualPos = sawingStopPos`, `speed = movingSpeed`
activate MoveToPos
MoveToPos -> MoveToPos : Check if `actualPos` < `requiredPos`

alt Start Timer if not running
    MoveToPos -> MoveToPos : Set `timer1start = TRUE`
    MoveToPos -> MoveToPos : Calculate `calculatedTime` based on `speed`
end

loop Until stop reaches required stop position
    MoveToPos -> MoveToPos : `timer1` regulates update interval
    alt Timer reached
        MoveToPos -> MoveToPos : Set `timer1start = FALSE`
        alt `requiredPos` < `actualPos`
            MoveToPos -> MoveToPos : Decrement `newpos = actualPos - 1`
        else `requiredPos` > `actualPos`
            MoveToPos -> MoveToPos : Increment `newpos = actualPos + 1`
        end
    else
        MoveToPos -> MoveToPos : Set `newpos = actualPos` (no change)
    end
    MoveToPos --> SLS : Return `newpos`
    SLS -> MoveToPos : Update `actualPos` with `newpos`
end

MoveToPos -> SLS : Final position reached (`actualPos = requiredPos`)
deactivate MoveToPos
SLS -> UI : Move product against stop

== Saw Task (Moving Saw to Bottom and Back) ==
UI -> Saw : Initiate sawing process
Saw -> MoveToPos : Set `requiredPos = bottomPos`, `actualPos = topPos`, `speed = gvl.sawingSpeed`
activate MoveToPos
MoveToPos -> MoveToPos : Check if `actualPos` < `requiredPos`

alt Start Timer if not running
    MoveToPos -> MoveToPos : Set `timer1start = TRUE`
    MoveToPos -> MoveToPos : Calculate `calculatedTime` based on `speed`
end

loop Until saw reaches `bottomPos`
    MoveToPos -> MoveToPos : `timer1` regulates update interval
    alt Timer reached
        MoveToPos -> MoveToPos : Set `timer1start = FALSE`
        alt `requiredPos` < `actualPos`
            MoveToPos -> MoveToPos : Decrement `newpos = actualPos - 1`
        else `requiredPos` > `actualPos`
            MoveToPos -> MoveToPos : Increment `newpos = actualPos + 1`
        end
    else
        MoveToPos -> MoveToPos : Set `newpos = actualPos` (no change)
    end
    MoveToPos --> Saw : Return `newpos`
    Saw -> MoveToPos : Update `actualPos` with `newpos`
end

MoveToPos -> Saw : Reached `bottomPos`, now returning to `topPos`
Saw -> MoveToPos : Set `requiredPos = topPos`, `actualPos = bottomPos`, `speed = gvl.sawingSpeed`

loop Until saw reaches `topPos`
    MoveToPos -> MoveToPos : `timer1` regulates update interval
    alt Timer reached
        MoveToPos -> MoveToPos : Set `timer1start = FALSE`
        alt `requiredPos` < `actualPos`
            MoveToPos -> MoveToPos : Decrement `newpos = actualPos - 1`
        else `requiredPos` > `actualPos`
            MoveToPos -> MoveToPos : Increment `newpos = actualPos + 1`
        end
    else
        MoveToPos -> MoveToPos : Set `newpos = actualPos` (no change)
    end
    MoveToPos --> Saw : Return `newpos`
    Saw -> MoveToPos : Update `actualPos` with `newpos`
end

MoveToPos -> Saw : Final position reached (`actualPos = requiredPos`)
deactivate MoveToPos
Saw -> UI : Notify sawing complete
@enduml

