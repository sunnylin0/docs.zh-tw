---
title: "組件定位 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-bcl"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "<codeBase> 項目"
  - "組件 [.NET Framework], 位置"
  - "組件 [.NET Framework], 位置"
  - "尋找組件"
ms.assetid: ff8d48bc-f606-484f-9fe1-d0af264269fb
caps.latest.revision: 9
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 9
---
# 組件定位
對於大部分 .NET Framework 應用程式，您可以將構成該應用程式的組件放置在應用程式的目錄、應用程式目錄的子目錄或全域組件快取 \(如果該組件是共用的\) 中。  您可以使用組態檔中的 [\<codeBase\> 項目](../../../docs/framework/configure-apps/file-schema/runtime/codebase-element.md)來覆寫 Common Language Runtime 尋找組件的位置。  如果組件不具有強式名稱 \(Strong Name\)，就會將使用 [\<codeBase\> 項目](../../../docs/framework/configure-apps/file-schema/runtime/codebase-element.md)指定的位置限制為應用程式目錄或子目錄。  如果組件具有強式名稱，[\<codeBase\> 項目](../../../docs/framework/configure-apps/file-schema/runtime/codebase-element.md)即可以指定電腦或網路上的任何位置。  
  
 使用 Unmanaged 程式碼或 COM Interop 應用程式時，可套用類似的規則來尋找組件：如果將會有多個應用程式共用這個組件，應該將它安裝在全域組件快取中。  配合 Unmanaged 程式碼使用的組件必須匯出為型別程式庫並且加以註冊。  COM Interop 所使用的組件必須在資料庫目錄 \(Catalog\) 中註冊，不過在某些狀況下這項註冊會自動進行。  
  
## 請參閱  
 [執行階段如何找出組件](../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md)   
 [設定應用程式](../../../docs/framework/configure-apps/index.md)   
 [Advanced COM Interoperability](http://msdn.microsoft.com/zh-tw/3ada36e5-2390-4d70-b490-6ad8de92f2fb)   
 [Common Language Runtime 中的組件](../../../docs/framework/app-domains/assemblies-in-the-common-language-runtime.md)