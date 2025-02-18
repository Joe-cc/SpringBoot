# 理賠作業系統流程圖
```mermaid
flowchart TB
    Start([開始]) --> Input[收件建檔]
    Input --> ValidateData{資料驗證}

    ValidateData -->|不完整| RequestDocs[要求補件]
    RequestDocs --> Input
    
    ValidateData -->|完整| CreateCase[生成案件編號]
    CreateCase --> AssignCase[案件分派]
  
    AssignCase --> ProcessType{處理類型}
    
    ProcessType -->|一般案件| NormalProcess[一般處理流程]
    ProcessType -->|可疑案件| Investigation[調查處理]
    ProcessType -->|重大案件| SpecialProcess[特殊處理流程]

    NormalProcess --> Review[初審作業]
    Investigation --> InvestigateProcess[調查作業]
    SpecialProcess --> MultiDeptReview[跨部門審核]

    Review --> CheckDocs{文件確認}
    CheckDocs -->|需補件| SendNotice[發送補件通知]
    SendNotice --> WaitDocs[等待補件]
    WaitDocs --> Review
 
    CheckDocs -->|完整| Calculate[理賠金額試算]

    InvestigateProcess --> Report[調查報告]
    Report --> Calculate

    MultiDeptReview --> Calculate
    
    Calculate --> AmountCheck{金額確認}
    AmountCheck -->|需覆核| SecondReview[覆核作業]
    AmountCheck -->|直接結案| Approve[核准理賠]
    
    SecondReview --> Approve
    
    Approve --> Payment[理賠金支付]
    Payment --> GenerateDocs[產生理賠文件]
    GenerateDocs --> Archive[歸檔作業]
    Archive --> End([結束])
   
    subgraph 系統功能模組
    DB[(資料庫)]
    API[API服務]
    Auth[認證授權]
    Report[報表系統]
    end
 
    subgraph 外部整合
    Payment -->|支付系統| PaymentSystem[金流系統]
    API -->|外部服務| ExternalServices[外部服務]
    end
