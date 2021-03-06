---
title: "&lt;connectionPoolSettings&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6fa7fa65-2c6e-4eab-b8cf-7690112c0be5
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# &lt;connectionPoolSettings&gt;
為具名管道繫結指定其他連線集區設定。  
  
## 語法  
  
```  
  
<connectionPoolSettings  
        groupName=”String”  
    idleTimeout"TimeSpan"  
    maxOutboundConnectionsPerEndpopint=”Integer” />  
```  
  
## 屬性和項目  
 下列章節說明屬性、子項目和父項目。  
  
### 屬性  
  
|屬性|描述|  
|--------|--------|  
|`groupName`|定義用於傳出通道之連線集區名稱的字串。  在資料流處理模式中，不會共用連線，意指會停用連線集區。  預設為 "default" 字串。  您可以修改這個值，將特定用戶端的連線隔離在不同群組中。|  
|`idleTimeout`|正的 <xref:System.TimeSpan>，指定連線在中斷連接之前，可以閒置的最長時間。  預設為 00:02:00。|  
|`maxOutboundConnectionsPerEndpoint`|正整數，指定與服務初始化之遠端端點連線的數目上限。  超過這個限制的連線都會進入佇列，直到低於此限制的空間可用為止。  `idleTimeout` 會限制在擲回例外狀況之前，連線保留在佇列中的持續期間。  預設值為 10。<br /><br /> 這個屬性會限制從用戶端到特定服務端點的同時作用中連線數目。  如果因為擁有更多個作用中的用戶端連線而超出這個值，對用戶端而言，服務可能會像是沒有回應。  在這種情況下，這個值應調整為超過預期的、用戶端對特定端點之同時連線的數目上限。|  
  
### 子項目  
 無。  
  
### 父項目  
  
|項目|描述|  
|--------|--------|  
|[\<namedPipeTransport\>](../../../../../docs/framework/configure-apps/file-schema/wcf/namedpipetransport.md)|定義傳輸，此傳輸會使用具名管道產生可傳輸訊息的通道。|  
  
## 請參閱  
 <xref:System.ServiceModel.Configuration.NamedPipeConnectionPoolSettingsElement>   
 <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement.ConnectionPoolSettings%2A>   
 <xref:System.ServiceModel.Channels.NamedPipeConnectionPoolSettings>   
 <xref:System.ServiceModel.Channels.TransportBindingElement>   
 <xref:System.ServiceModel.Channels.CustomBinding>   
 [傳輸](../../../../../docs/framework/wcf/feature-details/transports.md)   
 [選擇傳輸](../../../../../docs/framework/wcf/feature-details/choosing-a-transport.md)   
 [繫結](../../../../../docs/framework/wcf/bindings.md)   
 [擴充繫結](../../../../../docs/framework/wcf/extending/extending-bindings.md)   
 [自訂繫結](../../../../../docs/framework/wcf/extending/custom-bindings.md)   
 [\<customBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)