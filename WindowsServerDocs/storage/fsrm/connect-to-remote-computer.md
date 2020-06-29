---
title: 連線至遠端電腦
description: 本文說明如何連線至遠端電腦，以便從檔案伺服器資源管理員管理存放裝置資源
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 562164b461b4cd5db939b116feeb1bf21f78bad4
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474125"
---
# <a name="connect-to-a-remote-computer"></a>連線至遠端電腦

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

若要管理遠端電腦上的存放裝置資源，您可以從檔案伺服器資源管理員連線至該電腦。 當您連線時，[檔案伺服器資源管理員] 可讓您管理配額、檢測檔案、管理分類、排程檔案管理工作，以及使用那些遠端資源產生報告。

> [!Note]
> 檔案伺服器資源管理員可以管理本機電腦或遠端電腦上的資源，但無法兩者同時都管理。

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>若要從檔案伺服器資源管理員連線至遠端電腦

1.  在 **\[系統管理工具\]** 中，按一下 **\[檔案伺服器資源管理員\]**。

2.  在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[檔案伺服器資源管理員\]**，然後按一下 **\[連線到另一台電腦\]**。

3.  在 **\[連線到另一台電腦\]** 對話方塊中，按一下 **\[另一台電腦\]**。 然後輸入您想要連線到的伺服器名稱 (或按一下 **\[瀏覽\]** 搜尋遠端電腦)。

4.  按一下 [確定]****。

> [!Important]
> **\[連線到另一台電腦\]** 命令只有在您從 **\[系統管理工具\]** 中開啟 \[檔案伺服器資源管理員\] 時才能使用。 當您從 [伺服器管理員] 存取檔案伺服器資源管理員時，命令無法使用。

## <a name="additional-considerations"></a>其他考量

若要使用檔案伺服器資源管理員管理遠端資源：

-   您必須使用屬於遠端電腦中 **\[系統管理員\]** 群組成員的網域帳戶來登入本機電腦。
-   遠端電腦必須執行 Windows Server，而系統必須已安裝檔案伺服器資源管理員。
-   必須啟用遠端電腦上的 **\[遠端檔案伺服器資源管理員管理\]** 例外。 使用 [控制台] 中的 [Windows 防火牆] 來啟用此例外。

## <a name="additional-references"></a>其他參考

-   [管理遠端存放裝置資源](managing-remote-storage-resources.md)