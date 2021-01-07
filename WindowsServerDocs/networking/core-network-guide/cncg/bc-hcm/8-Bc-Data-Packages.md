---
title: 建立 Web 與檔案內容的內容伺服器資料套件 (選用)
description: 瞭解如何在 Web 和檔案伺服器上 prehash 內容，然後建立要在託管快取伺服器上匯入的資料套件。
manager: brianlic
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 638fc98b0d3271918d5d88df9cedd5d33429ad29
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965445"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>建立 Web 與檔案內容的內容伺服器資料套件 (選用)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程式來 prehash Web 和檔案伺服器上的內容，然後建立要在託管快取伺服器上匯入的資料套件。

此程式是選擇性的，因為您不需要在託管快取伺服器上 prehash 和預先載入內容。 如果您未預先載入內容，當用戶端透過 WAN 連線下載資料時，系統會自動將資料新增至託管快取。

此程式提供在檔案伺服器和網頁伺服器上 prehashing 內容的指示。 如果您沒有其中一種類型的內容伺服器，就不需要執行該內容伺服器類型的指示。

>[!IMPORTANT]
>執行此程式之前，您必須在內容伺服器上安裝並設定 BranchCache。 此外，如果您計畫在內容伺服器上變更伺服器密碼，則在預先雜湊內容之前，請先 \- 修改伺服器密碼，使先前 \- 產生的雜湊失效。

若要執行此程序，您必須是 Administrators 群組成員。

## <a name="to-create-content-server-data-packages"></a>建立內容伺服器資料封裝

1. 在每個內容伺服器上，找出您想要 prehash 並新增至資料封裝的資料夾和檔案。 識別或建立您稍後在此程式中要儲存資料套件的資料夾。

2. 在伺服器電腦上，以系統管理員許可權開啟 Windows PowerShell。

3. 視您擁有的內容伺服器類型而定，執行下列其中一項或兩項作業：

    > [!NOTE]
    > – Path 參數的值是您的內容所在的資料夾。 您必須將下列命令中的範例值取代為包含您想要 prehash 並新增至封裝之資料的內容伺服器上的有效資料夾位置。

    - 如果您想要 prehash 的內容是在檔案伺服器上，請輸入下列命令，然後按 ENTER。

        ```
        Publish-BCFileContent -Path D:\share -StageData
        ```

    -   如果您想要 prehash 的內容是在 Web 服務器上，請輸入下列命令，然後按 ENTER。

        ```
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```

4. 在每個內容伺服器上執行下列命令，以建立資料套件。 以 \( \\ 您在 \) 此程式開始時所識別或建立的位置，取代– Destination 參數的範例值 D： temp。

    ```
    Export-BCDataPackage –Destination D:\temp
    ```

5. 從內容伺服器存取您要預先載入內容之託管快取伺服器上的共用，並將資料封裝複製到託管快取伺服器上的共用。

若要繼續進行本指南，請參閱 [將託管快取伺服器上的資料封裝匯入 &#40;選擇性&#41;](9-Bc-Import-Data.md)。

