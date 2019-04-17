---
title: 建立 Content 網頁和檔案伺服器資料套件內容（可省略）
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f814bbac5c74081259d8eef6deda79d914bfec7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>建立 Content 網頁和檔案伺服器資料套件內容（可省略）

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程序 prehash content 網頁和檔案伺服器，並建立匯入裝載快取伺服器上的資料套件。 

此程序是選擇性的因為您不需要 content prehash 和預先載入您裝載快取的伺服器上。 如果您未進行預先載入 content，資料會裝載快取自動新增為戶端 WAN 連接到下載。

此程序提供 prehashing content 檔案伺服器和網頁伺服器上的指示。 如果您尚未其中一種內容伺服器，您不必執行該內容伺服器類型的指示操作。

>[!IMPORTANT]
>在執行此程序之前，您必須安裝並設定 BranchCache 內容伺服器上。 此外，如果您打算變更內容伺服器上的伺服器密碼，這樣之前 pre\ hashing 內容 – 修改伺服器機密失效 previously\ 自訂 hashes。

若要執行此程序，您必須是系統管理員群組成員。

## <a name="to-create-content-server-data-packages"></a>若要建立內容伺服器資料套件

1. 在每個內容伺服器，找出您想要 prehash 新增至資料套件檔案與資料夾。 找出，或建立的資料夾要稍後此程序中儲存您的資料的套件。

2. 伺服器在電腦上，請以系統管理員權限開放 Windows PowerShell。

3. 執行下列一或兩個動作，根據您所內容伺服器的類型：

    > [!NOTE]
    > 值 – 路徑參數是您的所在位置的資料夾。 您必須具有有效的資料夾位置取代範例值下方的命令中，包含您想要 prehash 新增至套件的資料內容伺服器上。
  
    - 如果您想要 prehash content 檔案伺服器上，輸入下列命令，，然後按 ENTER 鍵。

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   如果您想要 prehash content 網頁伺服器上，輸入下列命令，，然後按 ENTER 鍵。

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. 執行下列命令，每個您內容伺服器上建立的資料套件。 取代範例值 \(D:\\temp\) – 目的地參數的位置，您辨識或建立在此程序的開頭。

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. 從內容伺服器，存取您想要 content，預先載入您裝載快取伺服器上的共用並將資料套件複製到裝載快取的伺服器上的共用。

若要繼續使用此快速入門，請查看[上裝載快取伺服器與 #40; 選用和 #41; 匯入的資料套件](9-Bc-Import-Data.md).

