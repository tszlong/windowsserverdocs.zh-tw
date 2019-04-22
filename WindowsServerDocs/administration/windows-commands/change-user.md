---
title: change user
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d32c27e4b4c91b553efe3b55ab38181f51ed4d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817489"
---
# <a name="change-user"></a>change user

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更遠端桌面工作階段主機 (rd 工作階段主機) 伺服器的安裝模式。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
## <a name="syntax"></a>語法
```
change user {/execute | /install | /query}
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/ 執行|啟用的主目錄的.ini 檔案對應。 此為預設設定。|
|/install|停用.ini 檔案對應的主目錄。 所有的.ini 檔案讀取並寫入至系統目錄。 在 rd 工作階段主機伺服器上安裝應用程式時，您必須停用的.ini 檔案對應。|
|/query|顯示的.ini 檔案對應的目前設定。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   使用**變更使用者 /install**再安裝應用程式在系統目錄中建立應用程式的.ini 檔案。 建立使用者特定.ini 檔案時，這些檔案會用做為來源。 安裝應用程式之後, 使用**變更使用者 / 執行**還原為標準的.ini 檔案對應。
-   第一次執行應用程式，它會搜尋其.ini 檔案的主目錄。 如果.ini 檔案中的主目錄中，找不到，但系統目錄中，遠端桌面服務將.ini 檔案複製到主目錄中，確保每位使用者都有唯一的應用程式.ini 檔案的複本。 主目錄中，會建立任何新的.ini 檔案。
-   每位使用者都應該有唯一的應用程式的.ini 檔案的複本。 這可防止執行個體的不同的使用者可能具有不相容的應用程式設定 （例如，不同的預設目錄或螢幕解析度）。
-   當系統是在安裝模式 (亦即**變更使用者 /install**)，就會發生幾件事。 所建立的所有登錄項目會都遮蔽底下**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentversion\Terminal Server\Install**，在**\SOFTWARE**子機碼或**\MACHINE**子機碼。 子機碼新增至**HKEY_CURrenT_USER**複製到下**\SOFTWARE**子機碼，以及子機碼新增至**HKEY_LOCAL_MACHINE**複製到下**\機器**子機碼。 如果應用程式會查詢 Windows 目錄使用系統呼叫，例如 GetWindowsdirectory，rd 工作階段主機伺服器會傳回 systemroot 目錄。 如果任何.ini 檔項目使用所新增的系統呼叫，例如 WritePrivateProfileString，它們會新增至 systemroot 目錄底下的.ini 檔案。
-   當系統未傳回給執行模式 (亦即**變更使用者 / 執行**)，和應用程式嘗試讀取下的登錄項目**HKEY_CURrenT_USER**不存在，遠端桌面服務檢查金鑰的複本是否存在底下**\Terminal Server\Install**子機碼。 若是如此，則子機碼會複製到底下的適當位置**HKEY_CURrenT_USER**。 如果應用程式會嘗試從.ini 檔案不存在的讀取，遠端桌面服務會搜尋該.ini 檔案系統根目錄下。 如果.ini 檔案位於系統根，會將它複製到使用者的主目錄的 \Windows 子目錄。 如果應用程式會查詢 Windows 目錄，rd 工作階段主機伺服器會傳回使用者的主目錄的 \Windows 子目錄。
-   當您登入時，遠端桌面服務會檢查其系統的.ini 檔案是否比更新的電腦上的.ini 檔案。 如果系統版本較新，.ini 檔案取代或合併為較新版本。 這取決於 INISYNC 位元，0x40，設定這個.ini 檔案。 舊版的.ini 檔案重新命名為 Inifile.ctx。 如果系統登錄值之下**\Terminal Server\Install**子機碼會比您下的版本還新**HKEY_CURrenT_USER**，您的子機碼的版本會在刪除或取代為新的子機碼從**\Terminal Server\Install**。
## <a name="BKMK_examples"></a>範例
-   若要停用主目錄中的.ini 檔案對應，請輸入：
    ```
    change user /install
    ```
-   若要啟用的主目錄中的.ini 檔案對應，請輸入：
    ```
    change user /execute
    ```
-   若要顯示的.ini 檔案對應的目前設定，請輸入：
    ```
    change user /query
    ```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[變更](change.md)
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
