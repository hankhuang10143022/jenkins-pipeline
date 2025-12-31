graph TD
    User([開發者 Developer]) -- Git Push --> GitLab[GitLab <br/> 原始碼版本控制]
    
    subgraph CI_Pipeline [CI 持續整合]
        GitLab -- Trigger Webhook --> Jenkins[Jenkins <br/> Pipeline 控制器]
        Jenkins -- 1. Checkout --> Build[Build Docker Image <br/> Tagging]
        Build -- 2. Push Image --> ECR[(AWS ECR <br/> Image Registry)]
    end

    subgraph CD_Pipeline [CD 持續部署]
        Jenkins -- 3. Invoke --> Ansible[Ansible <br/> 自動化部署模組]
        Ansible -- SSH Connect --> Target[Target Host VM]
    end

    subgraph Host_Operations [主機端操作]
        Target -- 4. Docker Pull --> ECR
        Target -- 5. Container Update --> Service[[Running App]]
    end

    %% 樣式設定
    style User fill:#f9f,stroke:#333,stroke-width:2px
    style Jenkins fill:#f96,stroke:#333,stroke-width:2px
    style ECR fill:#ff9,stroke:#333
    style Target fill:#9f9,stroke:#333
