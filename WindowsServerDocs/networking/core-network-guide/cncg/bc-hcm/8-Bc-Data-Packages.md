---
title: 建立 Web 與檔案內容的內容伺服器資料套件 (選用)
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8cd284a83736d17859968947f381af171fd6bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817169"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>建立 Web 與檔案內容的內容伺服器資料套件 (選用)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用此程序預先雜湊網頁及檔案伺服器上的內容，然後再建立匯入您的託管快取伺服器上的資料套件。 

此程序是選擇性的因為您不需要 prehash 和預先載入內容在託管快取伺服器上。 如果您未預先載入內容，資料會加入至託管快取會自動用戶端會下載它透過 WAN 連線。

此程序提供內容檔案伺服器和 Web 伺服器上的預先雜湊處理的指示。 如果沒有其中一種類型的內容伺服器，您不必執行指示該內容的伺服器類型。

>[!IMPORTANT]
>執行此程序之前，您必須安裝和設定 BranchCache 內容伺服器上。 此外，如果您計劃變更內容的伺服器上的伺服器密碼，這樣做之前預先\-雜湊內容 – 修改伺服器密碼失效之前\-產生雜湊。

若要執行此程序，您必須是 Administrators 群組成員。

## <a name="to-create-content-server-data-packages"></a>若要建立資料套件的內容伺服器

1. 在每個內容伺服器上，找出您想要預先雜湊，並將資料封裝的檔案和資料夾。 識別或建立您要儲存您的資料套件，稍後在此程序的資料夾。

2. 伺服器電腦上，開啟 Windows PowerShell 系統管理員權限。

3. 執行下列一或兩個動作，根據您所擁有的內容伺服器的類型：

    > [!NOTE]
    > 值 – 路徑參數為您的內容所在的資料夾。 您必須取代下列命令中的範例值有效的資料夾位置包含您想要預先雜湊，並新增至套件的資料到內容伺服器。
  
    - 如果您想要預先雜湊內容檔案伺服器上，輸入下列命令，，然後按 ENTER 鍵。

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   如果您想要預先雜湊的內容是在 Web 伺服器上，輸入下列命令，，然後按 ENTER 鍵。

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. 在每個內容伺服器上執行下列命令，以建立資料套件。 取代範例值\(d:\\temp\) – 目的地參數的位置來識別，或建立此程序的開頭。

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. 從內容伺服器中，存取您想要預先載入內容，您的託管快取伺服器上的共用，並將資料套件複製到託管快取伺服器上的共用。

若要繼續進行本指南，請參閱[在託管快取伺服器上的匯入資料套件&#40;選擇性&#41;](9-Bc-Import-Data.md)。

