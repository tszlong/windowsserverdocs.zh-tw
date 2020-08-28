---
title: 核准-AutoaddDevices
description: 核准 AutoaddDevices 的參考文章，核准等待系統管理員核准的電腦。
ms.topic: reference
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b04537fecb0f498fc0e342157845a77b8558caa1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038246"
---
# <a name="approve-autoadddevices"></a>核准-AutoaddDevices

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

核准正在等待系統管理核准的電腦。 啟用自動新增原則時，在未知電腦之前需要系統管理核准 (未預先設置) 可以安裝映射。 您可以使用 [伺服器屬性] 頁面的 [ **PXE 回應** ] 索引標籤來啟用此原則。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>]
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/RequestId： {Request ID &#124; ALL}|指定指派給擱置電腦的要求識別碼。 指定 **all** 以核准所有擱置的電腦。|
|[/MachineName： <Device name> ]|指定要新增之電腦的名稱。 核准所有電腦時，您無法使用此選項。|
|[/OU： <DN of OU> ]|指定應在其中建立電腦帳戶物件 (OU) 之組織單位的分辨名稱。 例如： **OU = MyOU，CN = Test，dc = Domain，dc = com**。 預設位置是預設電腦的容器。|
|[/User： <Domain\User &#124; User@Domain>]|設定電腦帳戶物件的許可權，以將所需的許可權指派給指定的使用者。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要指派給指定使用者的許可權類型。<p>-   **JoinOnly** 需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。<br />-   **Full** 提供使用者的完整存取權，包括將電腦加入網域的許可權。|
|[/JoinDomain： {Yes &#124; No}]|指定是否應在作業系統安裝期間，將電腦加入網域作為此電腦帳戶。 預設值為 **[是]**。|
|[/ReferralServer： <Server name> ]|指定要聯繫的伺服器名稱，以使用一般檔案傳輸通訊協定 (tftp) 來下載網路開機程式和開機映射。|
|[/BootProgram： <Relative path> ]|指定來自 remoteInstall 資料夾的相對路徑，到這部電腦應接收的網路開機程式。 例如： **boot\x86\pxeboot.com**。|
|[/WdsClientUnattend： <Relative path> ]|將 remoteInstall 資料夾的相對路徑指定為自動安裝檔案，以自動執行 Windows 部署服務用戶端。|
|[/BootImagepath： <Relative path> ]|指定來自 remoteInstall 資料夾的相對路徑，到這部電腦應接收的開機映射。|
## <a name="examples"></a>範例
若要核准具有 RequestId 12 的電腦，請輸入：
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
若要核准 RequestID 為20的電腦，並使用指定的設定來部署映射，請輸入：
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:OU=Test,CN=company,DC=Domain,DC=Com /User:Domain\User1
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
若要核准所有擱置的電腦，請輸入：
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AutoaddDevices 命令](using-the-delete-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-get-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
