# Function sequense


```plantuml

@startuml
actor Task
participant "POU_MoveToPos" as MoveToPos

Task -> MoveToPos : Set `requiredPos`, `actualPos`, `speed`
activate MoveToPos
MoveToPos -> MoveToPos : Check if `actualPos` < `requiredPos`

alt Start Timer if not running
    MoveToPos -> MoveToPos : Set `timer1start = TRUE`
    MoveToPos -> MoveToPos : Calculate `calculatedTime` based on `speed`
end

loop Until `actualPos` reaches `requiredPos`
    MoveToPos -> MoveToPos : `timer1` regulates update interval
    alt Timer reached
        MoveToPos -> MoveToPos : Set `timer1start = FALSE`
        alt `requiredPos` < `actualPos`
            MoveToPos -> MoveToPos : Decrement `newpos` (`newpos = actualPos - 1`)
        else `requiredPos` > `actualPos`
            MoveToPos -> MoveToPos : Increment `newpos` (`newpos = actualPos + 1`)
        end
    else
        MoveToPos -> MoveToPos : Set `newpos = actualPos` (no change)
    end
    MoveToPos --> Task : Return `newpos`
    Task -> MoveToPos : Update `actualPos` with `newpos`
end

MoveToPos -> Task : Final position reached (`actualPos = requiredPos`)
deactivate MoveToPos
@enduml
