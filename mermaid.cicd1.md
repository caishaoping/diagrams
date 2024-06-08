flowchart LR
 subgraph release-bom.json["release-bom.json"]
    direction TB
        Comp1["Comp1"]
        Comp2["Comp2"]
        Comp3["Comp3"]
        Comp4["Comp4"]
        CompS["CompS"]
 end
 subgraph CI["CI pipeline"]
    direction TB
        CIComp1["Comp1"]
        CIComp2["Comp2"]
        CIComp3["Comp3"]
 end
 subgraph CI["New CI pipeline"]
    direction TB
        CIComp4["Comp4"]
 end 
 subgraph HighEnv["HigherEnv"]
    direction TB
        TEST
        ACPT
        ACNT
        PROD
        CONT
        CLVE
 end


  Nexus[(gitlab Package)]
  NexusRelease[(gitlab Release Package)]

    A["aUser"]-- Initial BOM --> CompS --Update-->Nexus
    CIComp1["CIComp1"]--Update BOM--> Comp1
    CIComp2["CIComp2"]--Update BOM--> Comp2  
    CIComp3["CIComp3"]--Update BOM--> Comp3
    CIComp4["CIComp4"]--Append BOM--> Comp4
    Comp1--Get-->Nexus
    Comp1--Update-->Nexus    
    Comp2--Get-->Nexus
    Comp2--Update-->Nexus    
    Comp3--Get-->Nexus
    Comp3--Update-->Nexus
    Comp4--Get-->Nexus
    Comp4--Add-->Nexus

    Nexus --Deploy--> DEVL
    Nexus --> Promote --> NexusRelease
    NexusRelease --Deploy-->TEST
    NexusRelease --Deploy-->ACPT
    NexusRelease --Deploy-->ACNT
    NexusRelease --Deploy-->PROD
    NexusRelease --Deploy-->CONT
    NexusRelease --Deploy-->CLVE
