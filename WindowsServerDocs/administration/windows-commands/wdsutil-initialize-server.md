---
title: wdsutil 初始化-伺服器
description: Wdsutil initialize-server 的參考文章，會在安裝伺服器角色之後設定 Windows 部署服務伺服器以供初始使用。
ms.topic: reference
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c61da4f608e825a7cb19c8fb80f8f4b3a5c26fed
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524273"
---
# <a name="wdsutil-initialize-server"></a>wdsutil 初始化-伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在安裝伺服器角色之後，設定 Windows 部署服務伺服器以供初始使用。 執行此命令之後，您應該使用 wdsutil 的 [ [新增映射命令](wdsutil-add-image.md) ] 命令，將影像加入至伺服器。
## <a name="syntax"></a>語法
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|remInst<Full path>|指定 remoteInstall 資料夾的完整路徑和名稱。 如果指定的資料夾不存在，則此選項會在執行命令時建立。 即使是在遠端電腦的情況下，您也應該一律輸入本機路徑。 例如： **D:\remoteInstall**。|
|/Authorize| (DHCP) 將動態主機控制通訊協定中的伺服器授權。 只有在啟用 DHCP rogue 偵測時，才需要此選項，這表示必須先在 DHCP 中授權 Windows 部署服務 PXE 伺服器，才能服務用戶端電腦。 請注意，預設會停用 DHCP rogue 偵測。|
## <a name="examples"></a>範例
若要初始化伺服器，並將 remoteInstall 共用資料夾設定為 F：磁片磁碟機，請輸入。
```
wdsutil /Initialize-Server /remInst:F:\remoteInstall
```
若要初始化伺服器，並將 remoteInstall 共用資料夾設定為 C：磁片磁碟機，請輸入。
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:C:\remoteInstall
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil disable-server 命令](wdsutil-disable-server.md)
- [wdsutil enable-server 命令](wdsutil-enable-server.md)
- [wdsutil get-server 命令](wdsutil-get-server.md)
- [wdsutil 設定-server 命令](wdsutil-set-server.md)
- [wdsutil 啟動-伺服器命令](wdsutil-start-server.md)
- [wdsutil stop-server 命令](wdsutil-stop-server.md)
- [wdsutil 解除初始化-server 命令](wdsutil-uninitialize-server.md)
