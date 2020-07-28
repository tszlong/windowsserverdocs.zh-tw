---
title: 關於使用 Windows Server Essentials ADK 的重要資訊
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: fa1cee0d12182303a4c125089f11b121b848d6bf
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181214"
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>關於使用 Windows Server Essentials ADK 的重要資訊

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

若要建立和自訂 Windows Server Essentials 的映射，您可以使用[windows 8 adk](https://go.microsoft.com/fwlink/?LinkId=248647)中的許多工具，但 WINDOWS 8 Adk 和 Windows SERVER Essentials ADK 之間有一些重要的差異。

 您應該注意下列重要差異：

-   **%windir%\setup\script\SetupComplete.cmd** 中的某些設定已變更。 如果要使用此命令，您可以新增其他命令行，但請勿移除現有的行。

## <a name="working-with-passwords"></a>使用密碼

-   系統管理員的密碼設定為 Admin@123 ，而且在 Install.wim\unattend.xml 中已啟用自動登入。 因此，在初始化設定伺服器期間，您不必多次重新輸入密碼。 如果您的卸除式媒體的根目錄中有自訂的 unattend.xml，此設定會被覆寫，而且您將需要設定密碼並且在啟動期間登入...

-   在初始設定期間，會提示使用者建立新的帳戶和密碼。 這個新帳戶會成為作業系統的網路系統管理員帳戶。 然後 Administrator 帳戶和自動登入會停用。 您可以藉由使用 cfg.ini 檔案來進行品質保證測試，自動化這個程序。

-   請參閱 [Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) 文件，以取得有關建立 unattend.xml 檔案的詳細資料。

## <a name="see-also"></a>另請參閱

 [使用 Windows Server ESSENTIALS ADK 消費者入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)[建立和自訂映射額外的](Creating-and-Customizing-the-Image.md)[自訂](Additional-Customizations.md)[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)

