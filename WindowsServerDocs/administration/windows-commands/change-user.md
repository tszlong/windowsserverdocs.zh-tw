---
title: change user
description: 變更使用者命令的參考文章，這會變更遠端桌面工作階段主機伺服器的安裝模式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42d6a5575ebf732a91477a425d93b10f3293e89e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922496"
---
# <a name="change-user"></a>change user

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更遠端桌面工作階段主機伺服器的安裝模式。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱[Windows Server 中遠端桌面服務的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
change user {/execute | /install | /query}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /execute | 讓 .ini 檔案能夠對應到主目錄。 這是預設值。 |
| /install | 停用 .ini 檔案與主目錄的對應。 所有 .ini 檔案都會讀取並寫入至系統目錄。 在遠端桌面工作階段主機伺服器上安裝應用程式時，您必須停用 .ini 檔案對應。 |
| /query | 顯示 .ini 檔案對應的目前設定。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在安裝應用程式之前，請先使用 [**變更使用者/install** ]，為系統目錄中的應用程式建立 .ini 檔案。 建立使用者特定的 .ini 檔案時，會使用這些檔案作為來源。 安裝應用程式之後，請使用 [**變更使用者/execute** ] 還原為標準 .ini 檔案對應。

- 第一次執行應用程式時，它會在主目錄中搜尋其 .ini 檔案。 如果在主目錄中找不到 .ini 檔案，但是在系統目錄中找到，遠端桌面服務將 .ini 檔案複製到主目錄，確保每個使用者都有一個應用程式 .ini 檔案的唯一複本。 所有新的 .ini 檔案都會建立在主目錄中。

- 每位使用者都應該有一個應用程式的 .ini 檔案的唯一複本。 這可防止不同使用者可能會有不相容的應用程式設定（例如，不同的預設目錄或螢幕解析度）。

- 當系統執行**變更使用者/install**時，會發生幾件事。 所有建立的登錄專案都會以**HKEY_LOCAL_MACHINE \Software\microsoft\windows server\ NT\Currentversion\Terminal Server\Install**為依據，在**\SOFTWARE**子機碼或**\MACHINE**子機碼中。 新增至**HKEY_CURRENT_USER**的子機碼會在**\SOFTWARE**子機碼下複製，而新增至**HKEY_LOCAL_MACHINE**的子機碼會複製到**\MACHINE**子機碼下。 如果應用程式使用系統呼叫（例如 GetWindowsdirectory）來查詢 Windows 目錄，rd 工作階段主機伺服器會傳回 systemroot 目錄。 如果使用系統呼叫（例如 WritePrivateProfileString）新增任何 .ini 檔案專案，則會將它們新增至 systemroot 目錄底下的 .ini 檔案。

- 當系統返回**變更使用者/execute**，而應用程式嘗試讀取**HKEY_CURRENT_USER**不存在的登錄專案時，遠端桌面服務檢查是否有金鑰複本存在於**\Terminal Server\Install**子機碼底下。 如果有，子機碼就會複製到**HKEY_CURRENT_USER**底下的適當位置。 如果應用程式嘗試從不存在的 .ini 檔案讀取，遠端桌面服務會在系統根目錄下搜尋該 .ini 檔案。 如果 .ini 檔案位於系統根目錄中，它會複製到使用者主目錄的 \Windows 子目錄中。 如果應用程式查詢 Windows 目錄，rd 工作階段主機伺服器會傳回使用者主目錄的 \Windows 子目錄。

- 當您登入時，遠端桌面服務會檢查其 system .ini 檔案是否比您電腦上的 .ini 檔案更新。 如果系統版本較新，則您的 .ini 檔案會被取代或與較新的版本合併。 這取決於是否為這個 .ini 檔案設定 INISYNC 位0x40。 您先前版本的 .ini 檔案已重新命名為 Inifile. ctx。 如果**\Terminal Server\Install**子機碼下的系統登錄值比**HKEY_CURRENT_USER**下的版本還新，則會刪除您的子機碼版本，並以**\Terminal Server\Install**中的新子機碼取代。

## <a name="examples"></a>範例

- 若要在主目錄中停用 .ini 檔對應，請輸入：

  ```
  change user /install
  ```

- 若要在主目錄中啟用 .ini 檔案對應，請輸入：

  ```
  change user /execute
  ```

- 若要顯示 .ini 檔案對應的目前設定，請輸入：

  ```
  change user /query
  ```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [變更命令](change.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
