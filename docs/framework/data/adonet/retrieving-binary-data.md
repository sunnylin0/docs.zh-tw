---
title: "擷取二進位資料 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 56c5a9e3-31f1-482f-bce0-ff1c41a658d0
caps.latest.revision: 5
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 5
---
# 擷取二進位資料
**DataReader** 的預設行為是一旦有整列資料可用時，便將內送資料載入為資料列。  但是二進位大型物件 \(BLOB\) 需要不同的處理方式，因為它們所包含的資料可能高達數 GB，無法包含在單一資料列內。  **Command.ExecuteReader** 方法具有多載，會採用 <xref:System.Data.CommandBehavior> 引數以修改 **DataReader** 的預設行為。  您可將 <xref:System.Data.CommandBehavior> 傳遞給 **ExecuteReader** 方法以修改 **DataReader** 的預設行為，這樣可在接收到資料時依序載入資料，而不必載入資料列。  這種方式相當適於載入 BLOB 或其他大型資料結構。  請注意，這個行為取決於您的資料來源。  例如，從 Microsoft Access 傳回 BLOB 會將整個正在載入的 BLOB 載入記憶體，而不是在接收時循序載入。  
  
 設定 **DataReader** 來使用 **SequentialAccess** 時，您必須要注意存取傳回欄位的順序。  **DataReader** 的預設行為 \(一旦有整列資料可用時立即將其載入\) 能讓您在讀取下個資料列前用任何順序存取傳回欄位。  但是使用 **SequentialAccess** 時，您就必須依序存取 **DataReader** 傳回的欄位。  例如，如果您的查詢傳回三個資料行，而第三個是 BLOB，則您必須在存取第三個欄位內的 BLOB 資料前，先把第一和第二個欄位的值傳回。  如果您在尚未存取第一和第二個欄位前先存取第三個欄位，便無法再使用第一和第二個欄位的值。  這是因為 **SequentialAccess** 已修改了 **DataReader** 來循序傳回資料，而 **DataReader** 讀過資料後，就無法取得資料。  
  
 存取 BLOB 欄位的資料時，請使用 **DataReader** 的 **GetBytes** 或 **GetChars** 具型別存取子，以資料填入陣列。  您也可以對字元資料使用 **GetString**，然而  為了節省系統資統，您可能不想要將整個 BLOB 值載入單一字串變數。  您可以指定傳回資料的特定緩衝區大小，以及從傳回資料讀取的第一個位元組或字元的開始位置。  **GetBytes** 和 **GetChars** 會傳回 `long` 值，表示傳回的位元組數或字元數。  如果您將 Null 陣列傳遞給 **GetBytes** 或 **GetChars**，則傳回的長整數 \(Long\) 值將會是 BLOB 內的總位元組數或總字元數。  您可以選擇將陣列內的索引指定為要讀取資料的開始位置。  
  
## 範例  
 下列範例從 Microsoft SQL Server 中的 **pubs** 範例資料庫，傳回發行者 ID 與商標。  發行者識別碼 \(`pub_id`\) 是一個字元欄位，而商標則是 BLOB 影像。  由於 **logo** 欄位是點陣圖，因此範例會使用 **GetBytes** 傳回二進位資料。  請注意，由於欄位必須被循序存取，所以您要先存取目前資料列的發行者 ID，再存取標誌。  
  
```vb  
' Assumes that connection is a valid SqlConnection object.  
Dim command As SqlCommand = New SqlCommand( _  
  "SELECT pub_id, logo FROM pub_info", connection)  
  
' Writes the BLOB to a file (*.bmp).  
Dim stream As FileStream                   
' Streams the binary data to the FileStream object.  
Dim writer As BinaryWriter                 
' The size of the BLOB buffer.  
Dim bufferSize As Integer = 100        
' The BLOB byte() buffer to be filled by GetBytes.  
Dim outByte(bufferSize - 1) As Byte    
' The bytes returned from GetBytes.  
Dim retval As Long                     
' The starting position in the BLOB output.  
Dim startIndex As Long = 0             
  
' The publisher id to use in the file name.  
Dim pubID As String = ""              
  
' Open the connection and read data into the DataReader.  
connection.Open()  
Dim reader As SqlDataReader = command.ExecuteReader(CommandBehavior.SequentialAccess)  
  
Do While reader.Read()  
  ' Get the publisher id, which must occur before getting the logo.  
  pubID = reader.GetString(0)  
  
  ' Create a file to hold the output.  
  stream = New FileStream( _  
    "logo" & pubID & ".bmp", FileMode.OpenOrCreate, FileAccess.Write)  
  writer = New BinaryWriter(stream)  
  
  ' Reset the starting byte for a new BLOB.  
  startIndex = 0  
  
  ' Read bytes into outByte() and retain the number of bytes returned.  
  retval = reader.GetBytes(1, startIndex, outByte, 0, bufferSize)  
  
  ' Continue while there are bytes beyond the size of the buffer.  
  Do While retval = bufferSize  
    writer.Write(outByte)  
    writer.Flush()  
  
    ' Reposition start index to end of the last buffer and fill buffer.  
    startIndex += bufferSize  
    retval = reader.GetBytes(1, startIndex, outByte, 0, bufferSize)  
  Loop  
  
  ' Write the remaining buffer.  
  writer.Write(outByte, 0 , retval - 1)  
  writer.Flush()  
  
  ' Close the output file.  
  writer.Close()  
  stream.Close()  
Loop  
  
' Close the reader and the connection.  
reader.Close()  
connection.Close()  
  
```  
  
```csharp  
// Assumes that connection is a valid SqlConnection object.  
SqlCommand command = new SqlCommand(  
  "SELECT pub_id, logo FROM pub_info", connection);  
  
// Writes the BLOB to a file (*.bmp).  
FileStream stream;                            
// Streams the BLOB to the FileStream object.  
BinaryWriter writer;                          
  
// Size of the BLOB buffer.  
int bufferSize = 100;                     
// The BLOB byte[] buffer to be filled by GetBytes.  
byte[] outByte = new byte[bufferSize];    
// The bytes returned from GetBytes.  
long retval;                              
// The starting position in the BLOB output.  
long startIndex = 0;                      
  
// The publisher id to use in the file name.  
string pubID = "";                       
  
// Open the connection and read data into the DataReader.  
connection.Open();  
SqlDataReader reader = command.ExecuteReader(CommandBehavior.SequentialAccess);  
  
while (reader.Read())  
{  
  // Get the publisher id, which must occur before getting the logo.  
  pubID = reader.GetString(0);    
  
  // Create a file to hold the output.  
  stream = new FileStream(  
    "logo" + pubID + ".bmp", FileMode.OpenOrCreate, FileAccess.Write);  
  writer = new BinaryWriter(stream);  
  
  // Reset the starting byte for the new BLOB.  
  startIndex = 0;  
  
  // Read bytes into outByte[] and retain the number of bytes returned.  
  retval = reader.GetBytes(1, startIndex, outByte, 0, bufferSize);  
  
  // Continue while there are bytes beyond the size of the buffer.  
  while (retval == bufferSize)  
  {  
    writer.Write(outByte);  
    writer.Flush();  
  
    // Reposition start index to end of last buffer and fill buffer.  
    startIndex += bufferSize;  
    retval = reader.GetBytes(1, startIndex, outByte, 0, bufferSize);  
  }  
  
  // Write the remaining buffer.  
  writer.Write(outByte, 0, (int)retval - 1);  
  writer.Flush();  
  
  // Close the output file.  
  writer.Close();  
  stream.Close();  
}  
  
// Close the reader and the connection.  
reader.Close();  
connection.Close();  
```  
  
## 請參閱  
 [Working with DataReaders](http://msdn.microsoft.com/zh-tw/126a966a-d08d-4d22-a19f-f432908b2b54)   
 [SQL Server 二進位和大數值資料](../../../../docs/framework/data/adonet/sql/sql-server-binary-and-large-value-data.md)   
 [ADO.NET Managed 提供者和資料集開發人員中心](http://go.microsoft.com/fwlink/?LinkId=217917)