---
title: "在 .NET Framework 4 工作流程中使用 Interop 活動 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 9bb747f0-eb33-4f70-84cd-317382372dcd
caps.latest.revision: 20
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 20
---
# 在 .NET Framework 4 工作流程中使用 Interop 活動
使用建立活動 [!INCLUDE[vstecwinfx](../../../includes/vstecwinfx-md.md)] 或 [!INCLUDE[netfx35_short](../../../includes/netfx35-short-md.md)] 可以用於 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 工作流程使用 <xref:System.Activities.Statements.Interop> 活動。 本主題提供使用概觀 <xref:System.Activities.Statements.Interop> 活動。  
  
> [!NOTE]
>   <xref:System.Activities.Statements.Interop> 活動不會顯示在工作流程設計工具的工具箱除非工作流程的專案具有其 **目標 Framework** 設定設為 **.Net Framework 4** 或更新版本。  
  
## <a name="using-the-interop-activity-in-net-framework-45-workflows"></a>在 .NET Framework 4.5 Workflows 中使用 Interop 活動  
 在本主題中，會建立一個 [!INCLUDE[netfx35_short](../../../includes/netfx35-short-md.md)] 活動程式庫，其中包含 `DiscountCalculator` 活動。  `DiscountCalculator` 計算折扣，根據購買金額，並且包含 <xref:System.Workflow.Activities.SequenceActivity> ，其中包含 <xref:System.Workflow.Activities.PolicyActivity>。  
  
> [!NOTE]
>   [!INCLUDE[netfx35_short](../../../includes/netfx35-short-md.md)] 本主題中所建立的活動會使用 <xref:System.Workflow.Activities.PolicyActivity> 實作活動邏輯。 不需要使用自訂 [!INCLUDE[netfx35_short](../../../includes/netfx35-short-md.md)] 活動或 <xref:System.Activities.Statements.Interop> 規則中的活動，即可使用 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 工作流程。 如需使用規則中的範例 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 工作流程，而不需使用 <xref:System.Activities.Statements.Interop> 活動，請參閱 [.NET Framework 4.5 中的原則活動](../../../docs/framework/windows-workflow-foundation/samples/policy-activity-in-net-framework-4-5.md) 範例。  
  
#### <a name="to-create-the-net-framework-35-activity-library-project"></a>若要建立 .NET Framework 3.5 活動程式庫專案  
  
1.  開啟 [!INCLUDE[vs_current_long](../../../includes/vs-current-long-md.md)] ，然後選取 **新增** 然後 **專案...** 從 **檔案** 功能表。  
  
2.  展開 **其他專案類型** 節點 **已安裝的範本** 窗格，然後選取 **Visual Studio 方案**。  
  
3.  選取 **空白方案** 從 **Visual Studio 方案** 清單。 型別 `PolicyInteropDemo` 中 **名稱** 方塊，然後按一下 **確定**。  
  
4.  以滑鼠右鍵按一下 **PolicyInteropDemo** 中 **方案總管] 中** ，然後選取 **新增** 然後 **新增專案...**。  
  
    > [!TIP]
    >  如果 **方案總管] 中** ] 視窗未顯示，請選取 **方案總管] 中** 從 **檢視** 功能表。  
  
5.  在 **已安裝的範本** 清單中，選取 **Visual C#** 然後 **工作流程**。 選取 **.NET Framework 3.5** 與.NET Framework 版本下拉式清單，然後選取 **工作流程活動程式庫** 從 **範本** 清單。  
  
6.  型別 `PolicyActivityLibrary` 中 **名稱** 方塊，然後按一下 **確定**。  
  
7.  以滑鼠右鍵按一下 **Activity1.cs** 中 **方案總管] 中** ，然後選取 **刪除**。 按一下 [ **確定** ] 以確認。  
  
#### <a name="to-create-the-discountcalculator-activity"></a>若要建立 DiscountCalculator 活動  
  
1.  以滑鼠右鍵按一下 **PolicyActivityLibrary** 中 **方案總管] 中** ，然後選取 **新增** 然後 **活動**。  
  
2.  選取 **活動 （程式碼分開置放）** 從 **Visual C# 項目** 清單。 型別 `DiscountCalculator` 中 **名稱** 方塊，然後按一下 **確定**。  
  
3.  以滑鼠右鍵按一下 **DiscountCalculator.xoml** 中 **方案總管] 中** ，然後選取 **檢視程式碼**。  
  
4.  將下列三個屬性加入至 `DiscountCalculator` 類別。  
  
    ```csharp  
    public partial class DiscountCalculator : SequenceActivity  
    {  
        public double Subtotal { get; set; }  
        public double DiscountPercent { get; set; }  
        public double Total { get; set; }  
    }  
    ```  
  
5.  以滑鼠右鍵按一下 **DiscountCalculator.xoml** 中 **方案總管] 中** ，然後選取 **檢視表設計工具**。  
  
6.  拖放到 **原則** 活動從 **Windows Workflow v3.0** 區段 **工具箱** 並將它放 **DiscountCalculator** 活動。  
  
    > [!TIP]
    >  如果 **工具箱** ] 視窗未顯示，請選取 **工具箱** 從 **檢視** 功能表。  
  
#### <a name="to-configure-the-rules"></a>若要設定規則  
  
1.  按一下新加入 **原則** 來選取它，如果未選取的活動。  
  
2.  按一下 [ **RuleSetReference** 屬性 **屬性** ] 視窗來選取它，然後按一下屬性右邊的省略符號按鈕。  
  
    > [!TIP]
    >  如果 **屬性** 看不到視窗中，選擇 [ **屬性] 視窗** 從 **檢視** 功能表。  
  
3.  選取 **按一下 [新增...**。  
  
4.  按一下 [ **新增規則**。  
  
5.  將下列運算式輸入到 **條件** 方塊。  
  
    ```  
    this.Subtotal >= 50 && this.Subtotal < 100  
    ```  
  
6.  將下列運算式輸入到 **Then 動作** 方塊。  
  
    ```  
    this.DiscountPercent = 0.075  
    ```  
  
7.  按一下 [ **新增規則**。  
  
8.  將下列運算式輸入到 **條件** 方塊。  
  
    ```  
    this.Subtotal >= 100  
    ```  
  
9. 將下列運算式輸入到 **Then 動作** 方塊。  
  
    ```  
    this.DiscountPercent = 0.15  
    ```  
  
10. 按一下 [ **新增規則**。  
  
11. 將下列運算式輸入到 **條件** 方塊。  
  
    ```  
    this.DiscountPercent > 0  
    ```  
  
12. 將下列運算式輸入到 **Then 動作** 方塊。  
  
    ```  
    this.Total = this.Subtotal - this.Subtotal * this.DiscountPercent  
    ```  
  
13. 將下列運算式輸入到 **Else 動作** 方塊。  
  
    ```  
    this.Total = this.Subtotal  
    ```  
  
14. 按一下 [ **確定** 關閉 **規則集編輯器** 對話方塊。  
  
15. 請確認新建立 <xref:System.Workflow.Activities.Rules.RuleSet> 中選取 **名稱** 清單，然後按 **確定**。  
  
16. 按下 CTRL+SHIFT+B 以建置方案。  
  
 下列程式碼範例顯示在本程序中加入至 `DiscountCalculator` 活動的規則。  
  
```  
Rule1: IF this.Subtotal >= 50 && this.Subtotal < 100   
       THEN this.DiscountPercent = 0.075   
  
Rule2: IF this. Subtotal >= 100   
       THEN this.DiscountPercent = 0.15   
  
Rule3: IF this.DiscountPercent > 0   
       THEN this.Total = this.Subtotal - this.Subtotal * this.DiscountPercent   
       ELSE this.Total = this.Subtotal  
```  
  
 當 <xref:System.Workflow.Activities.PolicyActivity> 執行時，這三個規則會評估並修改 `Subtotal`, ，`DiscountPercent`, ，和 `Total` 屬性值 `DiscountCalculator` 活動，以計算所需的折扣。  
  
## <a name="using-the-discountcalculator-activity-with-the-interop-activity"></a>搭配使用 DiscountCalculator 活動與 Interop 活動  
 若要使用 `DiscountCalculator` 活動內 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 工作流程， <xref:System.Activities.Statements.Interop> 活動使用。 本章節中兩個工作流程會建立，其中一個使用程式碼，另一個使用工作流程設計工具，示範如何使用 <xref:System.Activities.Statements.Interop> 活動，具有 `DiscountCalculator` 活動。 兩個工作流程會使用同樣的主應用程式。  
  
#### <a name="to-create-the-host-application"></a>若要建立主應用程式  
  
1.  以滑鼠右鍵按一下 **PolicyInteropDemo** 中 **方案總管] 中** ，然後選取 **新增**, ，然後 **新增專案...**。  
  
2.  請確認 **.NET Framework 4.5** 在.NET Framework 版本下拉式清單中，已選取，然後選取  **工作流程主控台應用程式** 從 **Visual C# 項目** 清單。  
  
3.  型別 `PolicyInteropHost` 到 **名稱** 方塊，然後按一下 **確定**。  
  
4.  以滑鼠右鍵按一下 **PolicyInteropHost** 中 **方案總管] 中** ，然後選取 **屬性**。  
  
5.  在 **目標 framework** 下拉式清單中，變更選取範圍 **.NET Framework 4 Client Profile** 至 **.NET Framework 4.5**。 按一下 [ **是** 確認。  
  
6.  以滑鼠右鍵按一下 **PolicyInteropHost** 中 **方案總管] 中** ，然後選取 **加入參考...**。  
  
7.  選取 **PolicyActivityLibrary** 從 **專案** ] 索引標籤上，按一下 [ **確定**。  
  
8.  以滑鼠右鍵按一下 **PolicyInteropHost** 中 **方案總管] 中** ，然後選取 **加入參考...**。  
  
9. 選取 **System.Workflow.Activities**, ，**System.workflow.activities**, ，然後 **System.workflow.componentmodel** 從 **.NET** ] 索引標籤上，按一下 [ **確定**。  
  
10. 以滑鼠右鍵按一下 **PolicyInteropHost** 中 **方案總管] 中** ，然後選取 **設定為啟始專案**。  
  
11. 按下 CTRL+SHIFT+B 以建置方案。  
  
### <a name="using-the-interop-activity-in-code"></a>在程式碼中使用 Interop 活動  
 在此範例中，工作流程定義會使用建立程式碼，其中包含 <xref:System.Activities.Statements.Interop> 活動和 `DiscountCalculator` 活動。 使用叫用此工作流程 <xref:System.Activities.WorkflowInvoker> 和規則評估的結果會寫入主控台使用 <xref:System.Activities.Statements.WriteLine> 活動。  
  
##### <a name="to-use-the-interop-activity-in-code"></a>若要在程式碼中使用 Interop 活動  
  
1.  以滑鼠右鍵按一下 **Program.cs** 中 **方案總管] 中** ，然後選取 **檢視程式碼**。  
  
2.  將下列 `using` 陳述式加入至檔案最上方。  
  
    ```csharp  
    using PolicyActivityLibrary;  
    ```  
  
3.  移除 `Main` 方法的內容，並以下列程式碼取代。  
  
    ```csharp  
    static void Main(string[] args)  
    {  
        CalculateDiscountUsingCodeWorkflow();  
    }  
    ```  
  
4.  在名為 `Program` 的 `CalculateDiscountUsingCodeWorkflow` 類別中建立新方法，其中包含下列程式碼。  
  
    ```csharp  
    static void CalculateDiscountUsingCodeWorkflow()  
    {  
        Variable<double> Subtotal = new Variable<double>  
        {  
            Default = 75.99,  
            Name = "Subtotal"  
        };  
  
        Variable<double> DiscountPercent = new Variable<double>  
        {  
            Name = "DiscountPercent"  
        };  
  
        Variable<double> Total = new Variable<double>  
        {  
            Name = "Total"  
        };  
  
        Activity wf = new Sequence  
        {  
            Variables = { Subtotal, DiscountPercent, Total },  
            Activities =   
            {  
                new Interop  
                {  
                    ActivityType = typeof(DiscountCalculator),  
                    ActivityProperties =   
                    {  
                        { "Subtotal", new InArgument<double>(Subtotal) },  
                        { "DiscountPercentOut", new OutArgument<double>(DiscountPercent) },  
                        { "TotalOut", new OutArgument<double>(Total) }  
                    }  
                },  
                new WriteLine  
                {  
                    Text =  new InArgument<string>(env =>   
                        string.Format("Subtotal: {0:C}, Discount {1}%, Total {2:C}",   
                        Subtotal.Get(env), DiscountPercent.Get(env) * 100, Total.Get(env)))  
                }  
            }  
        };  
  
        WorkflowInvoker.Invoke(wf);  
    }  
    ```  
  
    > [!NOTE]
    >   `Subtotal`, ，`DiscountPercent`, ，和 `Total` 屬性 `DiscountCalculator` 顯示活動的引數為 <xref:System.Activities.Statements.Interop> 活動，並繫結至本機工作流程變數 <xref:System.Activities.Statements.Interop> 活動的 <xref:System.Activities.Statements.Interop.ActivityProperties%2A> 集合。 `Subtotal` 新增為 <xref:System.Activities.ArgumentDirection> 引數因為 `Subtotal` 資料會流入 <xref:System.Activities.Statements.Interop> 活動，以及 `DiscountPercent` 和 `Total` 新增為 <xref:System.Activities.ArgumentDirection> 引數因為他們的資料流出 <xref:System.Activities.Statements.Interop> 活動。 請注意，這兩個 <xref:System.Activities.ArgumentDirection> 引數會以名稱 `DiscountPercentOut` 和 `TotalOut` 表示它們代表 <xref:System.Activities.ArgumentDirection> 引數。  `DiscountCalculator` 類型指定為 <xref:System.Activities.Statements.Interop> 活動的 <xref:System.Activities.Statements.Interop.ActivityType%2A>。  
  
5.  按 CTRL+F5 建置並執行應用程式。 將 `Subtotal` 的值替換成不同的值，測試出 `DiscountCalculator` 活動所提供的不同折扣層級。  
  
    ```csharp  
    Variable<double> Subtotal = new Variable<double>  
    {  
        Default = 75.99, // Change this value.  
        Name = "Subtotal"  
    };  
    ```  
  
### <a name="using-the-interop-activity-in-the-workflow-designer"></a>在工作流程設計工具中使用 Interop 活動  
 在這個範例中，會使用工作流程設計工具建立工作流程。 此工作流程有相同的功能與前一個範例中，除了比而不是使用 <xref:System.Activities.Statements.WriteLine> 活動顯示折扣，主應用程式時擷取並顯示折扣資訊在工作流程完成。 同時，這個工作流程不使用區域工作流程變數來包含資料，而是在工作流程設計工具中建立引數，並在叫用工作流程時從主機傳入值。  
  
##### <a name="to-host-the-policyactivity-using-a-workflow-designer-created-workflow"></a>若要使用工作流程設計工具建立的工作流程裝載 PolicyActivity  
  
1.  以滑鼠右鍵按一下 **Workflow1.xaml** 中 **方案總管] 中** ，然後選取 **刪除**。 按一下 [ **確定** ] 以確認。  
  
2.  以滑鼠右鍵按一下 **PolicyInteropHost** 中 **方案總管] 中** ，然後選取 **新增**, ，**新項目**。  
  
3.  展開 **Visual C# 項目** 節點，然後選取 **工作流程**。 選取 **活動** 從 **Visual C# 項目** 清單。  
  
4.  型別 `DiscountWorkflow` 到 **名稱** 方塊，然後按一下 **新增**。  
  
5.  按一下 [ **引數** 上顯示工作流程設計工具左下角的按鈕 **引數** 窗格。  
  
6.  按一下 [ **建立引數**。  
  
7.  型別 `Subtotal` 到 **名稱** 方塊中，選取 **中** 從 **方向** 下拉式清單中，選取 **Double** 從 **引數型別** 下拉式清單中，然後按下 ENTER 儲存引數。  
  
    > [!NOTE]
    >  如果 **Double** 不在 **引數型別** 下拉式清單中，選取 **瀏覽型別...**, ，型別 `System.Double` 中 **型別名稱** 方塊，然後按一下 **確定**。  
  
8.  按一下 [ **建立引數**。  
  
9. 型別 `DiscountPercent` 到 **名稱** 方塊中，選取 **出** 從 **方向** 下拉式清單中，選取 **Double** 從 **引數型別** 下拉式清單中，然後按下 ENTER 儲存引數。  
  
10. 按一下 [ **建立引數**。  
  
11. 型別 `Total` 到 **名稱** 方塊中，選取 **出** 從 **方向** 下拉式清單中，選取 **Double** 從 **引數型別** 下拉式清單中，然後按下 ENTER 儲存引數。  
  
12. 按一下 [ **引數** 上的 [關閉工作流程設計工具左下角的按鈕 **引數** 窗格。  
  
13. 拖放到 **順序** 活動從 **控制流程** 區段 **工具箱** 拖放到工作流程設計工具介面。  
  
14. 拖放到 **Interop** 活動從 **移轉** 區段 **工具箱** 並將它放 **順序** 活動。  
  
15. 按一下 [ **Interop** 活動 **按一下以瀏覽...** 索引標籤，輸入 **DiscountCalculator** 中 **型別名稱** 方塊，然後按一下 **確定**。  
  
    > [!NOTE]
    >  當 <xref:System.Activities.Statements.Interop> 活動加入至工作流程和 `DiscountCalculator` 類型指定為其 <xref:System.Activities.Statements.Interop.ActivityType%2A>, 、 <xref:System.Activities.Statements.Interop> 活動會公開三個 <xref:System.Activities.ArgumentDirection> 引數和三個 <xref:System.Activities.ArgumentDirection> 代表三個公用屬性的引數 `DiscountCalculator` 活動。  <xref:System.Activities.ArgumentDirection> 引數具有相同名稱的三個公用屬性，而三 <xref:System.Activities.ArgumentDirection> 引數具有相同的名稱與 **出** 附加至屬性名稱。 在下列步驟中，在先前步驟中建立的工作流程引數會繫結至 <xref:System.Activities.Statements.Interop> 活動的引數。  
  
16. 型別 `DiscountPercent` 到 **輸入 VB 運算式** 方塊右邊的 **Discountpercent** 屬性並按下 TAB 鍵。  
  
17. 型別 `Subtotal` 到 **輸入 VB 運算式** 方塊右邊的 **小計** 屬性並按下 TAB 鍵。  
  
18. 型別 `Total` 到 **輸入 VB 運算式** 方塊右邊的 **Total** 屬性並按下 TAB 鍵。  
  
19. 以滑鼠右鍵按一下 **Program.cs** 中 **方案總管] 中** ，然後選取 **檢視程式碼**。  
  
20. 將下列 `using` 陳述式加入至檔案最上方。  
  
    ```csharp  
    using System.Collections.Generic;  
    ```  
  
21. 註解 `CalculateDiscountInCode` 方法中對 `Main` 方法的呼叫，然後加入下列程式碼。  
  
    > [!NOTE]
    >  如果您未遵循前述程序，而出現預設的 `Main` 程式碼，請以下列程式碼取代 `Main` 的內容。  
  
    ```csharp  
    static void Main(string[] args)  
    {  
        CalculateDiscountUsingDesignerWorkflow();  
        //CalculateDiscountUsingCodeWorkflow();  
    }  
    ```  
  
22. 在名為 `Program` 的 `CalculateDiscountUsingDesignerWorkflow` 類別中建立新方法，其中包含下列程式碼。  
  
    ```csharp  
    static void CalculateDiscountUsingDesignerWorkflow()  
    {  
        double SubtotalValue = 125.99;  
        Dictionary<string, object> wfargs = new Dictionary<string, object>  
        {  
            {"Subtotal", SubtotalValue}  
        };  
  
        Activity wf = new DiscountWorkflow();  
  
        IDictionary<string, object> outputs =  
            WorkflowInvoker.Invoke(wf, wfargs);  
  
        Console.WriteLine("Subtotal: {0:C}, Discount {1}%, Total {2:C}",  
            SubtotalValue, (double)outputs["DiscountPercent"] * 100,  
            outputs["Total"]);  
    }  
    ```  
  
23. 按 CTRL+F5 建置並執行應用程式。 若要指定不同的 `Subtotal` 金額，請變更下列程式碼中 `SubtotalValue` 的值。  
  
    ```csharp  
    double SubtotalValue = 125.99; // Change this value.  
    ```  
  
## <a name="rules-features-overview"></a>規則功能概觀  
 [!INCLUDE[wf1](../../../includes/wf1-md.md)] 規則引擎可支援以優先順序為主的方式處理規則，並且支援向前鏈結。 規則可針對單一項目或集合中之項目進行評估。 如需規則概觀與特定規則功能的詳細資訊，請參閱下表。  
  
|規則功能|文件|  
|-------------------|-------------------|  
|規則概觀|[Windows Workflow Foundation 規則引擎簡介](http://go.microsoft.com/fwlink/?LinkID=152836)|  
|RuleSet|[工作流程中使用 Ruleset](http://go.microsoft.com/fwlink/?LinkId=178516) 和 <xref:System.Workflow.Activities.Rules.RuleSet>|  
|規則評估|[Ruleset 中的規則評估](http://go.microsoft.com/fwlink/?LinkId=178517)|  
|規則鏈結|[向前鏈結控制項](http://go.microsoft.com/fwlink/?LinkId=178518) 和 [規則的向前鏈結](http://go.microsoft.com/fwlink/?LinkId=178519)|  
|處理規則中的集合|[處理規則中的集合](http://go.microsoft.com/fwlink/?LinkId=178520)|  
|使用 PolicyActivity|[使用 PolicyActivity 活動](http://go.microsoft.com/fwlink/?LinkId=178521) 和 <xref:System.Workflow.Activities.PolicyActivity>|  
  
 工作流程中建立 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 不會使用所提供的規則功能的所有 [!INCLUDE[wf1](../../../includes/wf1-md.md)], ，例如宣告式活動條件及條件式活動，例如 <xref:System.Workflow.Activities.ConditionedActivityGroup> 和 <xref:System.Workflow.Activities.ReplicatorActivity>。 如有必要，這個功能可供以 [!INCLUDE[vstecwinfx](../../../includes/vstecwinfx-md.md)] 和 [!INCLUDE[netfx35_short](../../../includes/netfx35-short-md.md)] 建立的工作流程使用。 [!INCLUDE[crdefault](../../../includes/crdefault-md.md)][移轉指引](../../../docs/framework/windows-workflow-foundation//migration-guidance.md)。