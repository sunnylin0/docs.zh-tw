---
title: "Creating the GamePiece Class | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 37a27a86-ac1c-47be-b477-cb4b819459d3
caps.latest.revision: 9
author: "wadepickett"
ms.author: "wpickett"
manager: "wpickett"
caps.handback.revision: 9
---
# Creating the GamePiece Class
**GamePiece** 類別包含下列作業需要的所有功能：載入 Microsoft XNA 遊戲片段映像、追蹤與遊戲片段相關的滑鼠狀態、捕捉滑鼠、提供操作和慣性處理，以及提供當遊戲片段達到檢視區限制時的彈回功能。  
  
## 私用成員  
 在 **GamePiece** 類別頂端，會宣告數個私用成員。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_PrivateMembers](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_privatemembers)]  
  
## 公用屬性  
 其中三個私用成員會透過公用屬性公開。  **Scale** 和 **PieceColor** 屬性分別可讓應用程式指定比例和片段的色彩。  **Bounds** 屬性的出現可讓一個片段利用另一個片段的界限來呈現其本身，例如當一個片段應重疊另一個片段時。  下列程式碼範例顯示公用屬性的宣告。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_PublicProperties](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_publicproperties)]  
  
## 類別建構函式  
 **GamePiece** 類別的建構函式可接受下列參數：  
  
-   [SpriteBatch](http://msdn.microsoft.com/library/microsoft.xna.framework.graphics.spritebatch.aspx) 類型。  傳遞到這裡的參考會指派給私用成員 `spriteBatch`，並且在遊戲片段呈現其本身時，用來存取 [SpriteBatch.Draw](http://msdn.microsoft.com/library/microsoft.xna.framework.graphics.spritebatch.draw.aspx) 方法。  此外，[GraphicsDevice](http://msdn.microsoft.com/library/microsoft.xna.framework.graphics.spritebatch.graphicsdevice.aspx) 屬性可用來建立與遊戲片段相關聯的 [Texture](http://msdn.microsoft.com/library/microsoft.xna.framework.graphics.texture.aspx) 物件，並取得檢視區的大小，以偵測遊戲片段在何時遇到視窗界限，讓該片段可以彈回。  
  
-   字串，指定要用於遊戲片段的映像檔案名稱。  
  
 此建構函式也會建立 <xref:System.Windows.Input.Manipulations.ManipulationProcessor2D> 物件和 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D> 物件，並針對這些物件的事件建立事件處理常式。  
  
 下列程式碼顯示 **GamePiece** 類別的建構函式。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_Constructor](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_constructor)]  
  
## 捕捉滑鼠輸入  
 **UpdateFromMouse** 方法負責偵測當滑鼠位於遊戲片段界限內時，何時按下滑鼠按鈕，以及偵測何時放開滑鼠按鈕。  
  
 按下滑鼠左鍵時 \(當滑鼠位於片段界限\)，這個方法會設定旗標，指出這個遊戲片段已捕捉滑鼠，並開始操作處理。  
  
 操作處理一開始會建立 <xref:System.Windows.Input.Manipulations.Manipulator2D> 物件的陣列，並將其傳遞至 <xref:System.Windows.Input.Manipulations.ManipulationProcessor2D> 物件。  這會導致操作處理器評估操作工具 \(在此案例中為單一操作工具\)，並引發操作事件。  
  
 此外，也會儲存發生拖曳的點。  在之後的 <xref:System.Windows.Input.Manipulations.ManipulationProcessor2D.Delta> 事件期間，將會使用這個點來調整差異轉譯值，讓遊戲片段轉向拖曳點後面的字行中。  
  
 最後，此方法會傳回滑鼠捕捉的狀態。  如此可讓 [GamePieceCollection](../../../docs/framework/common-client-technologies/creating-the-gamepiececollection-class.md) 物件在有多個遊戲片段時，管理擷取動作。  
  
 下列程式碼顯示 **UpdateFromMouse** 方法。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_UpdateFromMouse](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_updatefrommouse)]  
  
## 處理操作  
 操作開始時，會引發 <xref:System.Windows.Input.Manipulations.ManipulationProcessor2D.Started> 事件。  此事件的處理常式會在慣性處理發生時加以停止，並將 *processInertia* 旗標設為 `false`。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_OnManipulationStarted](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_onmanipulationstarted)]  
  
 與操作相關聯的值變更時，會引發 <xref:System.Windows.Input.Manipulations.ManipulationProcessor2D.Delta> 事件。  此事件的處理常式會使用以事件引數傳遞的差異值，對遊戲片段的位置和旋轉值進行變更。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_OnManipulationDelta](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_onmanipulationdelta)]  
  
 與操作相關聯的所有操作工具 \(在此案例中為單一操作工具\) 被移除時，操作處理器會引發 <xref:System.Windows.Input.Manipulations.ManipulationProcessor2D.Completed> 事件。  此事件的處理常式開始進行慣性處理時，會將慣性處理器的初始速度，設為事件引數所報告的速度，並將 *processInertia* 旗標設為 `true`。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_OnManipulationCompleted](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_onmanipulationcompleted)]  
  
## 處理慣性  
 當慣性處理針對角度和線性速度、位置 \(轉譯\) 座標以及旋轉外推新的值時，會引發 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D.Delta> 事件。  此事件的處理常式會使用以事件引數傳遞的差異值，來修改遊戲片段的位置和旋轉。  
  
 如果新的座標導致遊戲片段移到檢視區界限之外，慣性處理的速度會反轉。  這會使遊戲片段從其遇到的檢視區界限彈開。  
  
 當 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D> 物件在執行外推時，您無法變更其屬性。  因此，在反轉 X 或 Y 速度時，事件處理常式會先呼叫 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D.Complete%2A> 方法來停止慣性。  然後它會指派新的初始速度值來做為目前的速度值 \(針對海綿行為進行調整\)，並將 *processInertia* 旗標設為 `true`。  
  
 下列程式碼顯示 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D.Delta> 事件的事件處理常式。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_OnInertiaDelta](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_oninertiadelta)]  
  
 當慣性處理完成時，慣性處理器會引發 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D.Completed> 事件。  此事件的處理常式會將 *processInertia* 旗標設為 `false`。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_OnInertiaCompleted](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_oninertiacompleted)]  
  
 截至目前為止，沒有任何存在的邏輯真正導致慣性外推發生。  這會在 **ProcessInertia** 方法中完成。  這個方法，從遊戲更新迴圈 \([Game.Update](http://msdn.microsoft.com/library/microsoft.xna.framework.game.update.aspx) 方法\) 重複呼叫，會檢查 *processInertia* 旗標是否設為 `true`；如果是，則會呼叫 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D.Process%2A> 方法。  呼叫這個方法會導致外推發生，並引發 <xref:System.Windows.Input.Manipulations.InertiaProcessor2D.Delta> 事件。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_ProcessInertia](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_processinertia)]  
  
 在呼叫其中一個 Draw 方法多載之前，不會實際呈現遊戲片段。  會從遊戲繪圖迴圈 \([Game.Draw](http://msdn.microsoft.com/library/microsoft.xna.framework.game.draw.aspx) 方法\) 重複呼叫這個方法的第一個多載。  這會呈現具有目前位置、旋轉和縮放比例的遊戲片段。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_Draw](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_draw)]  
  
## 其他屬性  
 **GamePiece** 類別使用三個私用屬性。  
  
1.  **Timestamp** – 取得操作和慣性處理器所要使用的時間戳記值。  
  
2.  **X** – 取得或設定遊戲片段的 X 座標。  設定時，可調整用於點擊測試的界限，以及操作處理器的樞紐分析位置。  
  
3.  **Y** – 取得或設定遊戲片段的 Y 座標。  設定時，可調整用於點擊測試的界限，以及操作處理器的樞紐分析位置。  
  
 [!code-csharp[ManipulationXNA#_GamePiece_PrivateProperties](../../../samples/snippets/csharp/VS_Snippets_Misc/manipulationxna/cs/gamepiece.cs#_gamepiece_privateproperties)]  
  
## 請參閱  
 [Manipulations and Inertia](../../../docs/framework/common-client-technologies/manipulations-and-inertia.md)   
 [Using Manipulations and Inertia in an XNA Application](../../../docs/framework/common-client-technologies/use-manipulations-and-inertia-in-an-xna-application.md)   
 [Creating the GamePieceCollection Class](../../../docs/framework/common-client-technologies/creating-the-gamepiececollection-class.md)   
 [Creating the Game1 Class](../../../docs/framework/common-client-technologies/creating-the-game1-class.md)