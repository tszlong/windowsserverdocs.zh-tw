---
title: change user
description: 變更使用者命令的參考文章，此命令會變更遠端桌面工作階段主機伺服器的安裝模式。
ms.topic: reference
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 59e290b3c80bfb85e5cef9ae3cffb20c121f71b6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629858"
---
# <a name="change-user"></a>change user

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更遠端桌面工作階段主機伺服器的安裝模式。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
change user {/execute | /install | /query}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /execute | 啟用 .ini 檔案對應至主目錄。 這是預設值。 |
| /install | 停用 .ini 檔案對應至主目錄。 所有 .ini 檔案都會讀取並寫入系統目錄。 在遠端桌面工作階段主機伺服器上安裝應用程式時，您必須停用 .ini 檔案對應。 |
| /query | 顯示 .ini 檔案對應的目前設定。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在安裝應用程式之前，請先使用 [ **變更使用者/install** ]，為系統目錄中的應用程式建立 .ini 檔案。 建立使用者專屬的 .ini 檔案時，會使用這些檔案作為來源。 安裝應用程式之後，請使用 **變更使用者/execute** 來還原為標準 .ini 檔案對應。

- 當您第一次執行應用程式時，它會在主目錄中搜尋其 .ini 檔案。 如果在主目錄中找不到 .ini 檔案，但在系統目錄中找不到 .ini 檔案，遠端桌面服務將 .ini 檔案複製到主目錄，以確保每位使用者都有一份唯一的應用程式 .ini 檔案。 系統會在主目錄中建立任何新的 .ini 檔案。

- 每個使用者都應該有一個適用于應用程式之 .ini 檔案的唯一複本。 這可防止不同使用者可能有不相容的應用程式設定 (例如，不同的預設目錄或螢幕解析度) 。

- 當系統正在執行 **變更使用者/install**時，會發生幾件事。 所有建立的登錄專案都會遮蔽于 **HKEY_LOCAL_MACHINE \Software\microsoft\windows NT\Currentversion\Terminal Server\Install**（在 **\SOFTWARE** 子機碼或 **\MACHINE** 子機碼中）。 新增至 **HKEY_CURRENT_USER** 的子機碼會在 **\SOFTWARE** 子機碼下複製，並新增至 **HKEY_LOCAL_MACHINE** 的子機碼會在 **\MACHINE** 子機碼下複製。 如果應用程式使用系統呼叫（例如 GetWindowsdirectory）查詢 Windows 目錄，rd 工作階段主機伺服器會傳回 systemroot 目錄。 如果使用系統呼叫（例如 WritePrivateProfileString）新增任何 .ini 檔案專案，則會將這些專案加入至 systemroot 目錄下的 .ini 檔案。

- 當系統返回以 **變更使用者/execute**，而且應用程式嘗試讀取 **HKEY_CURRENT_USER** 不存在的登錄專案時，遠端桌面服務會查看該金鑰的複本是否存在於 **\Terminal Server\Install** 子機碼下方。 如果有的話，會將子機碼複製到 **HKEY_CURRENT_USER**下的適當位置。 如果應用程式嘗試從不存在的 .ini 檔案讀取，遠端桌面服務在系統根目錄下搜尋該 .ini 檔案。 如果 .ini 檔在系統根目錄中，它會複製到使用者主目錄的 \Windows 子目錄中。 如果應用程式查詢 Windows 目錄，rd 工作階段主機伺服器會傳回使用者主目錄的 \Windows 子目錄。

- 當您登入時，遠端桌面服務檢查其 system .ini 檔案是否比您電腦上的 .ini 檔案更新。 如果系統版本較新，則會將您的 .ini 檔案取代或與較新的版本合併。 這取決於是否已針對此 .ini 檔案設定 INISYNC 位0x40。 您先前版本的 .ini 檔案會重新命名為 Inifile. ctx。 如果 **\Terminal Server\Install** 子機碼底下的系統登錄值比您在 **HKEY_CURRENT_USER**下的版本新，則會刪除您的子機碼，並將其取代為 **\Terminal Server\Install**中的新子機碼。

## <a name="examples"></a>範例

- 若要在主目錄中停用 .ini 檔案對應，請輸入：

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
