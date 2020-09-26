---
title: servermanagercmd
description: Servermanagercmd 命令的參考文章，此命令會安裝和移除角色、角色服務和功能。
ms.topic: reference
ms.assetid: 507c4b87-8e13-4872-8b34-0c7508eecbc1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 4a00294b70728a41cc68ae15d35a8c19fd768cf7
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389156"
---
# <a name="servermanagercmd"></a>servermanagercmd

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安裝和移除角色、角色服務和功能。 也會顯示可用的所有角色、角色服務和功能的清單，並顯示安裝在這部電腦上的角色。

> [!IMPORTANT]
> 此命令 servermanagercmd 已被取代，而且在未來的 Windows 版本中不保證會受到支援。 建議您改為使用可供伺服器管理員使用的 Windows PowerShell Cmdlet。 如需詳細資訊，請參閱[安裝或解除安裝角色、角色服務或功能](/administration/server-manager/install-or-uninstall-roles-role-services-or-features)。

## <a name="syntax"></a>語法

```
servermanagercmd -query [[[<drive>:]<path>]<query.xml>] [-logpath [[<drive>:]<path>]<log.txt>]
servermanagercmd -inputpath  [[[<drive>:]<path>]<answer.xml>] [-resultpath <result.xml> [-restart] | -whatif] [-logpath [[<drive>:]<path>]<log.txt>]
servermanagercmd -install <id> [-allSubFeatures] [-resultpath [[<drive>:]<path>]<result.xml> [-restart] | -whatif] [-logpath [[<Drive>:]<path>]<log.txt>]
servermanagercmd -remove <id> [-resultpath <result.xml> [-restart] | -whatif] [-logpath  [[<drive>:]<path>]<log.txt>]
servermanagercmd [-help | -?]
servermanagercmd -version
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -query `[[[<drive>:]<path>]<query.xml>]` | 顯示已安裝且可在伺服器上安裝之所有角色、角色服務和功能的清單。 您也可以使用此參數的簡短形式 **-q**。 如果您想要將查詢結果儲存至 XML 檔案，請指定要取代的 XML 檔 `<query.xml>` 。 |
| -inputpath  `[[[<drive>:]<path>]<answer.xml>]` | 安裝或移除以表示的 XML 回應檔案中指定的角色、角色服務和功能 `<answer.xml>` 。 您也可以使用此參數的簡短形式 **-p。** |
| -安裝 `<id>` | 安裝所指定的角色、角色服務或功能 `<id>` 。 識別碼不區分大小寫。 多個角色、角色服務和功能必須以空格分隔。 下列選用參數可與 **-install** 參數搭配使用：<ul><li>**-設定** `<SettingName>=<SettingValue>` -指定安裝所需的設定。</li><li>**-allSubFeatures** -指定所有次級服務和功能的安裝，以及在值中命名的父角色、角色服務或功能 `<id>` 。<p>**注意**<br>某些角色容器沒有可允許安裝所有角色服務的命令列識別碼。 如果角色服務不能安裝在與伺服器管理員命令的同一個例項中，就是這種情況。 例如，無法使用相同的伺服器管理員命令實例來安裝 active directory Federation Services 和同盟服務 Proxy 角色服務的同盟服務角色服務。</li><li>**-resultpath** `<result.xml>` -將安裝結果儲存至所代表的 XML 檔案 `<result.xml>` 。 您也可以使用此參數的簡短形式 **-r**。<p>**注意**<br>您無法使用指定的 **-resultpath** 參數和 **-whatif** 參數來執行 servermanagercmd。</li><li>**-重新開機** -當安裝完成時，自動重新開機電腦 (如果) 安裝的角色或功能需要重新開機。</li><li>**-whatif** -顯示為 **-install** 參數指定的任何作業。 您也可以使用 **-whatif** 參數的簡短形式 **-w**。 您無法使用指定的 **-resultpath**參數和 **-whatif**參數來執行**servermanagercmd** 。</li><li>**-logpath** `<[[<drive>:]<path>]<log.txt>>` -指定記錄檔的名稱和位置，而不是預設值 `%windir%\temp\servermanager.log` 。</li></ul> |
| -移除 `<id>` | 移除所指定的角色、角色服務或功能 `<id>` 。 識別碼不區分大小寫。 多個角色、角色服務和功能必須以空格分隔。 下列選擇性參數會搭配 **-remove** 參數使用：<ul><li>**-resultpath** `<[[<drive>:]<path>]result.xml>` -將移除結果儲存至所代表的 XML 檔案 `<result.xml>` 。 您也可以使用此參數的簡短形式 **-r**。<p>**注意**<br>您無法使用指定的 **-resultpath** 和 **-whatif** 參數來執行 servermanagercmd。</li><li>**-重新開機** -移除完成時自動重新開機電腦 (如果剩餘角色或功能) 需要重新開機。</li><li>**-whatif** -顯示為-remove 參數指定的任何作業。 您也可以使用-whatif 參數的簡短形式 **-w**。 您無法使用指定的 **-resultpath** 和 **-whatif** 參數來執行 servermanagercmd。</li><li>**-logpath** `<[[<Drive>:]<path>]<log.txt>>`-指定記錄檔的名稱和位置，而不是預設值 `%windir%\temp\servermanager.log` 。</li></ul> |
| -version | 顯示伺服器管理員的版本號碼。 您也可以使用簡短形式 **-v**。 |
| -help | 在 [命令提示字元] 視窗中顯示說明。 您也可以使用簡短形式 **-？**。 |

## <a name="examples"></a>範例

若要顯示所有角色、角色服務和功能的清單，以及安裝在電腦上的角色、角色服務和功能，請輸入：

```
servermanagercmd -query
```

若要安裝 Web 服務器 (IIS) 角色，然後將安裝結果儲存至 *installResult.xml*所代表的 XML 檔案，請輸入：

```
servermanagercmd -install Web-Server -resultpath installResult.xml
```

若要顯示有關要安裝或移除的角色、角色服務和功能的詳細資訊，請根據 *install.xml*所代表的 XML 回應檔案中指定的指示，輸入：

```
servermanagercmd -inputpath install.xml -whatif
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [伺服器管理員總覽](/administration/server-manager/server-manager)
