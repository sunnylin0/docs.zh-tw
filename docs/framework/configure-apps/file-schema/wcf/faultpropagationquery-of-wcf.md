---
title: "WCF 的 &lt;faultPropagationQuery&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: fabafbc8-3e45-4feb-8321-0725e9f4079c
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# WCF 的 &lt;faultPropagationQuery&gt;
代表查詢，這個查詢可用來追蹤活動中發生之錯誤的處理。這個事件會在每次 FaultHandler 處理錯誤時發生。  您應該使用這種查詢來追蹤活動中發生的錯誤處理。  追蹤參與者必須要具備查詢，才能訂閱錯誤傳播記錄。  
  
 如需追蹤設定檔查詢的詳細資訊，請參閱[追蹤設定檔](../../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md)。  
  
## 語法  
  
```vb  
  
<tracking>  
   <trackingProfile name="Name">  
       <workflow>  
          <faultPropagationQueries>  
             <faultPropagationQuery activityName="String"  
                 faultHandlerActivityName="String"/>  
          </faultPropagationQueries>  
       </workflow>  
   </trackingProfile>  
</tracking>  
  
```  
  
## 屬性和項目  
 下列章節說明屬性、子項目和父項目。  
  
### 屬性  
  
|屬性|描述|  
|--------|--------|  
|activityName|字串，可指定傳用錯誤之錯誤處理常式活動的名稱。  預設為 \*，表示會針對所有活動傳回錯誤傳用記錄。|  
|faultHandlerActivityName|字串，可指定本身是錯誤來源的活動名稱。|  
  
### 子項目  
 無。  
  
### 父項目  
  
|項目|描述|  
|--------|--------|  
|[\<faultPropagationQueries\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/faultpropagationqueries.md)|代表組態項目的清單，可用來追蹤活動中發生之錯誤的處理。這個事件會在每次 FaultHandler 處理錯誤時發生。|  
  
## 請參閱  
 [System.ServiceModel.Activities.Tracking.Configuration.FaultPropagationQueryElement](assetId:///System.ServiceModel.Activities.Tracking.Configuration.FaultPropagationQueryElement?qualifyHint=False&amp;autoUpgrade=True)   
 [System.Activities.Tracking.FaultPropagationQuery](assetId:///System.Activities.Tracking.FaultPropagationQuery?qualifyHint=False&amp;autoUpgrade=True)   
 [工作流程追蹤與追查](../../../../../docs/framework/windows-workflow-foundation//workflow-tracking-and-tracing.md)   
 [追蹤設定檔](../../../../../docs/framework/windows-workflow-foundation//tracking-profiles.md)