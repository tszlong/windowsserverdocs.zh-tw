---
title: 使用核准 AutoaddDevices 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce9824c45a00ccb9f1f9e357c7e3d36b2857f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886819"
---
# <a name="using-the-approve-autoadddevices-command"></a>使用核准 AutoaddDevices 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

核准為暫止的系統管理核准的電腦。 啟用自動新增原則時，系統管理員核准後未知的電腦 （其尚未預先設置） 可以安裝映像。 您可以使用此原則啟用**PXE 回應** 索引標籤的 伺服器屬性 頁面。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/RequestId:{Request ID &#124; ALL}|指定擱置電腦的要求 ID。 指定**所有**核准擱置中的所有電腦。|
|[/ MachineName:<Device name>]|指定要新增之電腦的名稱。 核准所有電腦時，您無法使用此選項。|
|[/ OU:<DN of OU>]|指定組織單位 (OU)，應該在其中建立電腦帳戶物件的辨別的名稱。 例如: **OU = MyOU，CN = 測試，DC = Domain，DC = com**。 預設位置是在預設電腦容器。|
|[/ 使用者： < 網域 \ 使用者&#124; User@Domain>]|若要將指定的使用者指派的必要權限的電腦帳戶物件上設定權限。|
|[/ JoinRights: {JoinOnly&#124;完整}]|指定要指派給指定使用者的權限的類型。<br /><br />-   **JoinOnly**需要系統管理員使用者可以將電腦加入網域之前，重設電腦帳戶。<br />-   **完整**可完整存取權提供給使用者，其中包含將電腦加入網域的權限。|
|[/JoinDomain:{Yes &#124; No}]|指定在電腦應加入網域成為這個電腦帳戶在作業系統安裝期間。 預設值是**是**。|
|[/ReferralServer:<Server name>]|指定要連線來使用簡單式檔案傳輸通訊協定 (tftp) 下載網路開機程式和開機映像的伺服器名稱。|
|[/BootProgram:<Relative path>]|指定應該會收到這台電腦的網路開機程式從 remoteInstall 資料夾的相對路徑。 例如： **boot\x86\pxeboot.com**。|
|[/WdsClientUnattend:<Relative path>]|指定自動安裝檔案，用來自動化 Windows 部署服務用戶端從 remoteInstall 資料夾的相對路徑。|
|[/BootImagepath:<Relative path>]|指定應該會收到這台電腦的開機映像從 remoteInstall 資料夾的相對路徑。|
## <a name="BKMK_examples"></a>範例
若要核准 requestid 為 12 個電腦，請輸入：
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
若要核准 requestid 為 20 個電腦，並部署映像，使用指定的設定，請輸入：
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:"OU=Test,CN=company,DC=Domain,DC=Com" /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
若要核准所有擱置電腦，請輸入：
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 delete AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)
[使用 get AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
 [使用拒絕 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
