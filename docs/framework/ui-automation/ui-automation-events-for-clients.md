---
title: "用戶端的 UI 自動化事件 | Microsoft Docs"
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
  - "UI 自動化中，用戶端的事件"
  - "UI 自動化用戶端的事件"
ms.assetid: b909e388-3f24-4997-b6d4-bd9c35c2dc27
caps.latest.revision: 32
author: "Xansky"
ms.author: "mhopkins"
manager: "markl"
caps.handback.revision: 32
---
# 用戶端的 UI 自動化事件
> [!NOTE]
>  這份文件適用於.NET Framework 開發人員想要使用 managed[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中定義的類別<xref:System.Windows.Automation>命名空間。 如需最新資訊[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]，請參閱[Windows Automation API: UI 自動化](http://go.microsoft.com/fwlink/?LinkID=156746)。  
  
 本主題說明如何[!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]事件由 UI 自動化用戶端。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]允許用戶端訂閱感興趣的事件。 這項功能可改善效能，不需要持續輪詢所有[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]以查看是否有變更任何資訊、 結構或狀態系統中的項目。  
  
 同時也因為能夠只接聽定義範圍內的事件，而改善效率。 例如，用戶端可以接聽所有的焦點變更事件[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]樹狀目錄中，或只有一個項目和其子系的項目。  
  
> [!NOTE]
>  不會假設所有可能的事件會引發由[!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]提供者。 例如，並非所有的屬性變更會造成的標準 proxy 提供者所引發的事件[!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)]和[!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)]控制項。  
  
 如需更多檢視[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]事件，請參閱 < [UI 自動化事件概觀](../../../docs/framework/ui-automation/ui-automation-events-overview.md)。  
  
<a name="Subscribing_to_Events"></a>   
## <a name="subscribing-to-events"></a>訂閱事件  
 用戶端應用程式藉由使用下列方法之一註冊事件處理常式，來訂閱特定種類的事件。  
  
|方法|事件類型|事件引數類型|委派類型|  
|------------|----------------|--------------------------|-------------------|  
|<xref:System.Windows.Automation.Automation.AddAutomationFocusChangedEventHandler%2A>|焦點變更|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|<xref:System.Windows.Automation.AutomationFocusChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A>|屬性變更|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|<xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddStructureChangedEventHandler%2A>|結構變更|<xref:System.Windows.Automation.StructureChangedEventArgs>|<xref:System.Windows.Automation.StructureChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A>|所有其他事件，由<xref:System.Windows.Automation.AutomationEvent>|<xref:System.Windows.Automation.AutomationEventArgs>或<xref:System.Windows.Automation.WindowClosedEventArgs>|<xref:System.Windows.Automation.AutomationEventHandler>|  
  
 在呼叫方法之前，您必須建立委派方法來處理事件。 您想要的話也可以在單一方法中處理不同類型的事件，並在對資料中其一個方法的多次呼叫裡傳遞此方法。 例如，單一<xref:System.Windows.Automation.AutomationEventHandler>可以設定以處理各種事件，以不同的方式根據以<xref:System.Windows.Automation.AutomationEventArgs.EventId%2A>。  
  
> [!NOTE]
>  若要處理的視窗關閉事件，轉型會傳遞至事件處理常式做為引數型別<xref:System.Windows.Automation.WindowClosedEventArgs>。 因為[!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]視窗的項目已不再有效，您無法使用`sender`參數來擷取資訊; 使用<xref:System.Windows.Automation.WindowClosedEventArgs.GetRuntimeId%2A>改。  
  
> [!CAUTION]
>  如果您的應用程式可能會接收來自自己的事件[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]，請勿使用您的應用程式[!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]執行緒事件、 訂閱或取消訂閱。 這樣可能會導致無法預期的行為。 如需詳細資訊，請參閱[UI 自動化執行緒問題](../../../docs/framework/ui-automation/ui-automation-threading-issues.md)。  
  
 關閉時，或當[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]事件不會再感興趣的應用程式中，UI 自動化用戶端應該呼叫下列方法之一。  
  
|方法|描述|  
|------------|-----------------|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationEventHandler%2A>|取消註冊事件處理常式所註冊的使用<xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A>。|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationFocusChangedEventHandler%2A>|取消註冊事件處理常式所註冊的使用<xref:System.Windows.Automation.Automation.AddAutomationFocusChangedEventHandler%2A>。|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationPropertyChangedEventHandler%2A>|取消註冊事件處理常式所註冊的使用<xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A>。|  
|<xref:System.Windows.Automation.Automation.RemoveAllEventHandlers%2A>|移除註冊所有已註冊的事件處理常式。|  
  
 範例程式碼，請參閱[訂閱 UI 自動化事件](../../../docs/framework/ui-automation/subscribe-to-ui-automation-events.md)。  
  
## <a name="see-also"></a>另請參閱  
 [訂閱 UI 自動化事件](../../../docs/framework/ui-automation/subscribe-to-ui-automation-events.md)   
 [UI 自動化事件概觀](../../../docs/framework/ui-automation/ui-automation-events-overview.md)   
 [UI 自動化屬性概觀](../../../docs/framework/ui-automation/ui-automation-properties-overview.md)   
 [TrackFocus 範例](http://msdn.microsoft.com/zh-tw/4a91c0af-6bb5-4d38-a743-cf136f268fc9)