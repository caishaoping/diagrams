flowchart LR
 subgraph subgraph["CD Pipeline "]
    subgraph release-bom.json["release-bom.json"]
        direction TB
            Comp1["Comp1"]
            Comp2["Comp2"]
            Comp3["Comp3"]
            Comp4["Comp4"]
                
    end
 end  
 subgraph Group["CI Build "]
    direction TB          
    subgraph CI["CI pipeline"]
        direction TB
            CIComp1["Comp1"]
            CIComp2["Comp2"]
            CIComp3["Comp3"]
            CIComp4["Comp4"]
    end
 end
 subgraph HighEnv["HigherEnv"]
    direction TB
        TEST
        ACPT
        ACNT
        PROD
        CONT
 end
 subgraph Promotes["Promote"]
    direction TB
        Promote1
        Promote2
        Promote3
        Promote4
        Promote      
 end 

  CI-Artifact-Nexus[(Nexus CI artifactssnapshots for Comp1 and Comp2)]
  CI-Artifact-Gitlab[(Gitlab CI artifacts snapshots for Comp3 and Comp4)]  
  Nexus[(gitlab snapshots Package for release-bom)]
  Nexus1[(gitlab snapshots Package for release-bom)]  
  NexusRelease[(gitlab Release for Release-bom)]
  LegacyCI[(Nexus CI artifactssnapshots for Comp1 and Comp2)]
  NewCI[(Gitlab CI artifacts snapshots for Comp3 and Comp4)]
  LegacyCI0[(Nexus CI artifacts snapshots for Comp1 and Comp2)]
  NewCI0[(Gitlab CI artifacts snapshots for Comp3 and Comp4)]  
  CI-Artifact-Gitlab-Release[(Gitlab Release for CI artifacts for Comp3 and Comp4)]
  CI-Artifact-Nexus-Release[(Nexus Release for CI artifacts for Comp1 and Comp2)]

    CIComp1["CIComp1"] -- Upload to --> CI-Artifact-Nexus --Update BOM(artifact in Nexus)--> Comp1
    CIComp2["CIComp2"] -- Upload to --> CI-Artifact-Nexus --Update BOM(artifact in Nexus)--> Comp2  
    CIComp3["CIComp3"] -- Upload to --> CI-Artifact-Gitlab --Update BOM(artifact in Gitlab)--> Comp3
    CIComp4["CIComp4"] -- Upload to --> CI-Artifact-Gitlab --Update BOM(artifact in Gitlab)--> Comp4       
    Comp1--Get-->Nexus
    Comp1--Update-->Nexus    
    Comp2--Get-->Nexus
    Comp2--Update-->Nexus    
    Comp3--Get-->Nexus
    Comp3--Update-->Nexus
    Comp4--Get-->Nexus
    Comp4--Add-->Nexus

    style CI-Artifact-Gitlab color:#FF0000
    style NewCI color:#FF0000
    style NewCI0 color:#FF0000
    style CI-Artifact-Gitlab-Release color:#FF0000           
    style Nexus color:#FF0000
    style Nexus1 color:#FF0000
    style NexusRelease color:#FF0000
    style Promote3 color:#FF0000   
    style Promote4 color:#FF0000 
    style Promote color:#FF0000 

    Nexus --Deploy--> DEVL
    LegacyCI0 --Deploy--> DEVL
    NewCI0 --Deploy--> DEVL   
    Nexus --> Create-Release --> Nexus1 --> Promote --CreatedRelease--> NexusRelease

    NexusRelease --PreDeploy-->TFE-RUN

    LegacyCI --> Promote1 -- Comp1 --> CI-Artifact-Nexus-Release
    LegacyCI --> Promote2 -- Comp2 --> CI-Artifact-Nexus-Release
    NewCI --> Promote3 -- Comp3 --> CI-Artifact-Gitlab-Release
    NewCI --> Promote4 -- Comp4 --> CI-Artifact-Gitlab-Release

 
    CI-Artifact-Gitlab-Release --PreDeploy-->TFE-RUN
    CI-Artifact-Nexus-Release --PreDeploy-->TFE-RUN
    TFE-RUN --Deploy-->TEST
    TFE-RUN --Deploy-->ACPT
    TFE-RUN --Deploy-->ACNT
    TFE-RUN --Deploy-->PROD
    TFE-RUN --Deploy-->CONT
    
