---
title: 建立 Web 與檔案內容的內容伺服器資料套件 (選用)
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 104e3cfd0525c43857bb37d781f6b2475978238e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406382"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>建立 Web 與檔案內容的內容伺服器資料套件 (選用)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程式來預先雜湊 Web 和檔案伺服器上的內容，然後建立要在託管快取伺服器上匯入的資料封裝。 

這個程式是選擇性的，因為您不需要在託管快取伺服器上預先雜湊和預先載入內容。 如果您未預先載入內容，則會自動將資料新增至託管快取，因為用戶端會透過 WAN 連線進行下載。

此程式提供在檔案伺服器和網頁伺服器上 prehashing 內容的指示。 如果您沒有這種類型的內容伺服器，就不需要執行該內容伺服器類型的指示。

>[!IMPORTANT]
>執行此程式之前，您必須先在內容伺服器上安裝和設定 BranchCache。 此外，如果您計畫變更內容伺服器上的伺服器密碼，請在預先\-雜湊內容之前先執行此動作-修改伺服器密碼會使先前\-產生的雜湊失效。

若要執行此程序，您必須是 Administrators 群組成員。

## <a name="to-create-content-server-data-packages"></a>建立內容伺服器資料封裝

1. 在每個內容伺服器上，找出您想要預先雜湊並新增至資料封裝的資料夾和檔案。 在此程式中，識別或建立您要儲存資料封裝的資料夾。

2. 在伺服器電腦上，以系統管理員許可權開啟 Windows PowerShell。

3. 視您擁有的內容伺服器類型而定，執行下列其中一項或兩項操作：

    > [!NOTE]
    > – Path 參數的值是內容所在的資料夾。 您必須將下列命令中的範例值取代為內容伺服器上的有效資料夾位置，其中包含您想要預先雜湊並新增至套件的資料。
  
    - 如果您要預先雜湊的內容是在檔案伺服器上，請輸入下列命令，然後按 ENTER。

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   如果您要預先雜湊的內容是在 Web 服務器上，請輸入下列命令，然後按 ENTER。

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. 在每個內容伺服器上執行下列命令，以建立資料封裝。 以您在此程式開頭所識別或建立的位置取代-Destination 參數 \(D：\\temp\) 的範例值。

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. 從內容伺服器，存取您想要預先載入內容之託管快取伺服器上的共用，並將資料套件複製到託管快取伺服器上的共用。

若要繼續進行本指南，請參閱在託管快取[伺服器&#40;上匯&#41;入資料套件（選擇性](9-Bc-Import-Data.md)）。

