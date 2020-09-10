---
title: 步驟 3：將電腦加入新的 Windows Server Essentials 伺服器
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 7495831c6f593b65261fda8f50d4ef9d1000d9b4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625483"
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>步驟 3：將電腦加入新的 Windows Server Essentials 伺服器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials

遷移程式的下一個步驟是將用戶端電腦連線到執行 Windows Server Essentials 的新伺服器。

> [!NOTE]
>  執行 Windows XP 或 Windows Vista 作業系統的電腦可以略過這個步驟。 Windows Server 連接器軟體不支援執行 Windows XP 或 Windows Vista 的電腦。

 將用戶端電腦加入新的 Windows Server Essentials 伺服器之前，您必須先卸載用戶端電腦上的 Windows Server 連接器軟體，將用戶端電腦與來源伺服器中斷連線。

### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>解除安裝用戶端電腦上的 Windows Server 連接器

1.  從用戶端電腦開啟 [控制台]，然後開啟 [程式和功能]****。

2.  在程式清單中，以滑鼠右鍵按一下在電腦上執行的連接器應用程式。

    > [!NOTE]
    >  連接器應用程式可以是 **Windows Small Business Server 2011 Essentials 連接器**或 **Windows server Essentials 連接器**，視用戶端電腦連線到的 windows server essentials 版本而定。

3.  按一下 [解除安裝]  。

### <a name="to-reconnect-a-client-computer-to-the-server"></a>將用戶端電腦重新連線至伺服器

1.  登入到您想要連線到伺服器的電腦。

    > [!NOTE]
    >  如果此電腦有多個使用者帳戶，請使用在您將此電腦連線至伺服器後，要保留其文件、圖片和個人喜好設定的使用者帳戶登入。

2.  開啟網際網路瀏覽器，例如 Internet Explorer。

3.  在網址列中，輸入 **HTTP://<servername \> /Connect**，然後按 enter。

4.  遵循畫面上的指示，將用戶端電腦加入新的 Windows Server Essentials 伺服器。

## <a name="next-steps"></a>後續步驟
 您已將用戶端電腦加入至執行 Windows Server Essentials 的新伺服器。 現在移至 [步驟4：將設定和資料移至目的地伺服器以進行 Windows Server Essentials 遷移](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。


若要查看所有步驟，請參閱 [遷移至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

