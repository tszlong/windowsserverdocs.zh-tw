---
title: Servermanagercmd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 507c4b87-8e13-4872-8b34-0c7508eecbc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: ba0b85814d942323b12e1874b852fcf28b8ac068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883239"
---
# <a name="servermanagercmd"></a>Servermanagercmd

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

> [!IMPORTANT]
> 只有在執行 Windows Server 2008 的伺服器或 Windows Server 2008 R2 上使用此命令。 **Servermanagercmd.exe**已被取代，並不是適用於 Windows Server 2012。 如需有關如何安裝或移除 Windows Server 2012 中的角色、 角色服務與功能的資訊，請參閱 <<c0> [ 安裝或解除安裝角色、 角色服務與功能](https://go.microsoft.com/fwlink/?LinkID=239563)Microsoft TechNet 上。

安裝和移除角色、 角色服務與功能。 也會顯示的所有角色、 角色服務和可用的功能清單，並顯示 這台電腦上安裝的。 如需有關角色、 角色服務和功能，您可以指定使用此工具的詳細資訊，請參閱[伺服器管理員說明](https://go.microsoft.com/fwlink/?LinkID=137387)。 如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法
```
servermanagercmd -query [[[<Drive>:]<path>]<query.xml>] [-logpath   [[<Drive>:]<path>]<log.txt>]
servermanagercmd -inputpath  [[<Drive>:]<path>]<answer.xml> [-resultpath <result.xml> [-restart] | -whatif] [-logpath [[<Drive>:]<path>]<log.txt>]
servermanagercmd -install <Id> [-allSubFeatures] [-resultpath   [[<Drive>:]<path>]<result.xml> [-restart] | -whatif] [-logpath   [[<Drive>:]<path>]<log.txt>]
servermanagercmd -remove <Id> [-resultpath    <result.xml> [-restart] | -whatif] [-logpath  [[<Drive>:]<path>]<log.txt>]
servermanagercmd [-help | -?]
servermanagercmd -version
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|-query [[[\<Drive>:]\<path>]\<*query.xml*>]|在伺服器上顯示所有角色、 角色服務和已安裝且可供安裝的功能的清單。 您也可以使用這個參數的簡短形式 **-q**。 如果您想將查詢結果儲存至 XML 檔案，指定 XML 檔案來取代*query.xml*。|
|-inputpath  <[[\<Drive>:]\<path>]*answer.xml*>|安裝或移除角色、 角色服務和功能所代表的 XML 回應檔案中指定*answer.xml*。 您也可以使用這個參數的簡短形式 **-p。**|
|-安裝\<*識別碼*>|安裝角色、 角色服務或所指定的功能*識別碼*。識別碼不區分大小寫。 必須以空格分隔多個角色、 角色服務與功能。 下列選用參數會搭配 **-安裝**參數。<br /><br />-   **-設定** \< *SettingName*>=\<*SettingValue*> 安裝指定所需的設定。<br />-   **-allSubFeatures**指定安裝所有下層服務與功能以及父系角色、 角色服務或功能命名*識別碼*值。 **注意：**    有些角色容器並沒有命令列識別碼，以允許安裝所有角色服務。 角色服務無法安裝在相同的執行個體的伺服器管理員命令時，這會是大小寫。 比方說，active directory Federation Services，Federation Service Proxy 角色服務的 Federation Service 角色服務無法使用相同的伺服器管理員命令執行個體安裝。<br />-   **-resultpath** \< *result.xml > 將安裝結果儲存至所代表的 XML 檔案*result.xml*。您也可以使用這個參數的簡短形式 **-r**。**注意：**   您無法執行**servermanagercmd**兼具 **-resultpath**參數和 **-whatif**所指定的參數。<br />-    **-重新啟動**電腦會自動重新啟動時安裝已完成 （如果角色或功能安裝需要重新啟動）。<br />-    **-whatif**會顯示指定的任何操作 **-安裝**參數。您也可以使用的簡稱 **-whatif**參數 **-w**。您無法執行**servermanagercmd**兼具 **-resultpath**參數和 **-whatif**所指定的參數。<br />-    **-logpath** \<[[\<磁碟機 >:]\<路徑 >]* log.txt* > 指定的名稱和記錄檔，預設以外的位置 **%windir%\temp\servermanager.log**。|
|-移除\<*識別碼*>|移除角色、 角色服務或所指定的功能*識別碼*。識別碼不區分大小寫。 必須以空格分隔多個角色、 角色服務與功能。 下列選用參數會搭配 **-移除**參數。<br /><br />-   **-resultpath** \<[[\<磁碟機 >:]\<路徑 >]*result.xml*> 將移除結果儲存至 XML 檔案所代表*result.xml*。 您也可以使用這個參數的簡短形式 **-r**。 **注意：**    您無法執行**servermanagercmd**兼具 **-resultpath**參數， **-whatif**指定參數。<br />-   **-重新啟動**電腦會自動重新啟動時移除已完成 （如果剩餘的角色或功能需要重新啟動）。<br />-   **-whatif**會顯示指定的任何操作 **-移除**參數。 您也可以使用的簡稱 **-whatif**參數 **-w**。 您無法執行**servermanagercmd**兼具 **-resultpath**參數， **-whatif**指定參數。<br />-   **-logpath**\<[[\<磁碟機 >:]\<路徑 >]*log.txt*> 指定的名稱和記錄檔，預設以外的位置 **%windir%\temp\servermanager.log**。|
|-help|在命令提示字元 視窗中顯示說明。 您也可以使用簡短形式 **-？**。|
|-version|顯示伺服器管理員版本號碼。 您也可以使用簡短形式 **-v**。|

## <a name="remarks"></a>備註
**Servermanagercmd**已經過時，而且不保證在未來的 Windows 版本中支援。 我們建議您在執行 Windows Server 2008 R2 的電腦上執行伺服器管理員，如果您使用的 Windows PowerShell cmdlet，可以使用 伺服器管理員。 如需詳細資訊，請參閱 <<c0> [ 伺服器管理員 cmdlet](https://go.microsoft.com/fwlink/?LinkID=137653)。
在伺服器的本機磁碟機上，可以從任何目錄執行 Servermanagercmd。 您必須是您想要安裝或移除軟體的伺服器上的 Administrators 群組的成員。

> [!IMPORTANT]
> 由於 Windows Server 2008 R2 中的使用者帳戶控制 設有安全性限制，您必須執行**Servermanagercmd**使用提高的權限開啟命令提示字元視窗中。 若要這樣做，請以滑鼠右鍵按一下命令提示字元可執行檔，或**命令提示字元**物件上**開始**功能表，然後再按一下**系統管理員身分執行**。

## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**servermanagercmd**顯示一份所有角色、 角色服務和功能，哪些角色、 角色服務與功能安裝在電腦上。
```
servermanagercmd -query
```
下列範例示範如何使用**servermanagercmd**安裝網頁伺服器 (IIS) 角色，並將安裝結果儲存至所代表的 XML 檔案*installResult.xml*。
```
servermanagercmd -install Web-Server -resultpath installResult.xml
```
下列範例示範如何使用 * * whatif * * 參數搭配**servermanagercmd**來顯示詳細的資訊的角色、 角色服務和功能，您會安裝或移除，根據指示進行，所代表的 XML 回應檔案中指定*install.xml*。
```
servermanagercmd -inputpath install.xml -whatif
```

#### <a name="additional-references"></a>其他參考資料
-   如需完整的角色、 角色服務或功能識別碼清單中，您可以指定針對*識別碼*參數或使用 XML 回應檔案的詳細資訊**Servermanagercmd**，請參閱[伺服器管理員說明](https://go.microsoft.com/fwlink/?LinkID=137387)。 (https://go.microsoft.com/fwlink/?LinkID=137387).
-   請參閱[伺服器管理員 cmdlet](https://go.microsoft.com/fwlink/?LinkID=137653)如需可用的伺服器管理員的 Windows PowerShell cmdlet 的清單。
-   [命令列語法關鍵](command-line-syntax-key.md)
