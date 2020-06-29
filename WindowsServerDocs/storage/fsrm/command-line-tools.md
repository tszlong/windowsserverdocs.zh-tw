---
title: 檔案伺服器資源管理員命令列工具
description: 本文說明 Windows Server 2016 命令列工具
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2acce64aa14d60503a5b443b831a03338c204be2
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472814"
---
# <a name="file-server-resource-manager-command-line-tools"></a>檔案伺服器資源管理員命令列工具

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

檔案伺服器資源管理員會安裝 [FileServerResourceManager](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager) PowerShell Cmdlet，以及下列命令列工具：

-   **Dirquota.exe**。 用於建立和管理配額，自動套用配額及配額範本。
-   **Filescrn.exe**。 用於建立和管理檔案檢測、檔案檢測範本、檔案檢測例外及檔案群組。
-   **Storrept.exe**。 用於設定報告參數，以及依需求產生存放裝置報告。 也用於建立報告工作，您可以使用 **schtasks.exe** 來排程這些工作。

您可以使用這些工具，來管理在本機電腦或遠端電腦上的儲存資源。 如需這些命令列工具的詳細資訊，請參閱下列參考資料：

-   **Dirquota**：<https://go.microsoft.com/fwlink/?LinkId=92741>
-   **Filescrn**：<https://go.microsoft.com/fwlink/?LinkId=92742>
-   **Storrept**：<https://go.microsoft.com/fwlink/?LinkId=92743>


> [!Note]
> 若要查看命令語法以及命令的可用參數，請使用 <strong>/?</strong> 參數。


## <a name="remote-management-using-the-command-line-tools"></a>使用命令列工具進行遠端管理

每個工具都有數個用於執行動作的選項，這些動作與那些在檔案伺服器資源管理員 MMC 嵌入式管理單元中提供的動作類似。 若要讓命令對遠端電腦，而不是對本機電腦執行動作，請使用 **/remote**:*ComputerName* 參數。

例如，**Dirquota.exe** 包含 **template export** 參數便可將配額範本設定寫入 XML 檔案，而包含 **template import** 參數則可從 XML 檔案匯入範本設定。 將 **/remote**:*ComputerName* 參數加入 **Dirquota.exe template import** 命令，就會將範本從本機電腦上的 XML 檔案匯入至遠端電腦。

> [!Note]
> 當您執行加上 **/remote**:<em>ComputerName</em> 參數的命令列工具，對遠端電腦執行範本匯出 (或匯入) 時，會在本機電腦上的 XML 檔案中寫入範本，或複製其中的範本。

<br />

## <a name="additional-considerations"></a>其他考量

若要使用命令列工具管理遠端資源：

-   您必須使用屬於本機電腦或遠端電腦中 **\[系統管理員\]** 群組成員的網域帳戶來登入。
-   您必須從提高權限的 [命令提示字元] 視窗執行命令列工具。 若要開啟提升權限的命令提示字元視窗，請按一下 [開始]****，依序指向 [所有程式]**** 及 [附屬應用程式]****，以滑鼠右鍵按一下 [命令提示字元]****，然後按一下 [以系統管理員身分執行]****。
-   遠端電腦必須執行 Windows Server，而系統必須已安裝檔案伺服器資源管理員。
-   必須啟用遠端電腦上的 **\[遠端檔案伺服器資源管理員管理\]** 例外。 使用 [控制台] 中的 [Windows 防火牆] 來啟用此例外。


## <a name="additional-references"></a>其他參考

-   [管理遠端存放裝置資源](managing-remote-storage-resources.md)