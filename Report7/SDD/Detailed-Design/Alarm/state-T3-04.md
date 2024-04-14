```mermaid
stateDiagram
    [*] --> Triggered
    Triggered --> Sent: All action \nexecuted successfully
  
    Triggered --> Solved: Condition is \nnot met anymore?
    Sent --> Solved:  Condition is \nnot met anymore?
    Solved --> [*]
```