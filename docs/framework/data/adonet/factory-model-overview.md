---
title: "Factory 模型概觀 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b5dc81c4-7554-44b9-b513-769bd61e2e7b
caps.latest.revision: 4
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 4
---
# Factory 模型概觀
ADO.NET 2.0 在 <xref:System.Data.Common> 命名空間 \(Namespace\) 中導入了新的基底類別 \(Base Class\)。  這些基底類別是抽象類別，表示它們無法直接具現化 \(Instantiated\)。  它們包括 <xref:System.Data.Common.DbConnection>、<xref:System.Data.Common.DbCommand> 和 <xref:System.Data.Common.DbDataAdapter>，而且可由 .NET Framework 資料提供者 \(例如 <xref:System.Data.SqlClient> 和 <xref:System.Data.OleDb>\) 共用。  加入基底類別可簡化針對 .NET Framework 資料提供者加入功能的程序，而且不需要建立新的介面。  
  
 ADO.NET 2.0 也導入了抽象基底類別，可讓開發人員撰寫不需仰賴特定資料提供者的泛型資料存取程式碼。  
  
## Factory 設計模式  
 撰寫獨立於提供者以外之程式碼的程式設計模型是以 "Factory" 設計模式為基礎，而這種設計模式會使用單一 API 來存取多個提供者之間的資料庫。  要適當命名此模式，因為它會要求僅用特定的物件來建立其他物件，與實際的 Factory 非常相似。  如需 Factory 設計模式的更詳細說明，請參閱 MSDN 上的＜[在 ASP.NET 2.0 和 ADO.NET 2.0 中撰寫泛型資料存取程式碼](http://go.microsoft.com/fwlink/?LinkId=55915)＞和＜使用 ADO.NET 2.0 基底類別和 Factory 進行泛型編碼＞[http:\/\/msdn.microsoft.com\/library\/default.asp?url\=\/library\/dnvs05\/html\/vsgenerics.asp](http://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/vsgenerics.asp)。  
  
 從 ADO.NET 2.0 開始，<xref:System.Data.Common.DbProviderFactories> 類別會提供 `static` \(或 Visual Basic 中的 `Shared`\) 方法來建立 <xref:System.Data.Common.DbProviderFactory> 執行個體。  然後，此執行個體會根據提供者資訊以及在執行階段提供的連接字串，傳回正確的強型別物件。  
  
## 請參閱  
 [取得 DbProviderFactory](../../../../docs/framework/data/adonet/obtaining-a-dbproviderfactory.md)   
 [DbConnection、DbCommand 和 DbException](../../../../docs/framework/data/adonet/dbconnection-dbcommand-and-dbexception.md)   
 [使用 DbDataAdapter 來修改資料](../../../../docs/framework/data/adonet/modifying-data-with-a-dbdataadapter.md)   
 [ADO.NET Managed 提供者和資料集開發人員中心](http://go.microsoft.com/fwlink/?LinkId=217917)