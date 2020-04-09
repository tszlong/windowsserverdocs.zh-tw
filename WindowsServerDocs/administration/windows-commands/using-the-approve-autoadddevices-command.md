---
title: 核准-AutoaddDevices
description: 核准的 Windows 命令主題-AutoaddDevices，可核准擱置系統管理核准的電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7f63faabf37337136cad186fd252915adf1a743
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831831"
---
# <a name="approve-autoadddevices"></a>核准-AutoaddDevices

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

核准等待系統管理核准的電腦。 啟用自動新增原則時，必須先進行系統管理核准，才會有未知的電腦（未預先設置的電腦）可以安裝映射。 您可以使用 [伺服器] 屬性頁的 [ **PXE 回應**] 索引標籤來啟用此原則。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/RequestId： {Request ID &#124; ALL}|指定指派給擱置電腦的要求識別碼。 指定 [**全部**] 以核准所有擱置中的電腦。|
|[/MachineName：<Device name>]|指定要新增之電腦的名稱。 核准所有電腦時，您無法使用此選項。|
|[/OU：<DN of OU>]|指定應該在其中建立電腦帳戶物件的組織單位（OU）的辨別名稱。 例如： **OU = MyOU，CN = Test，dc = Domain，DC = com**。 預設位置是預設電腦的容器。|
|[/User： < Domain\User &#124; User@Domain>]|設定電腦帳戶物件的許可權，將所需的許可權指派給指定的使用者。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要指派給指定使用者的許可權類型。<p>-   **JoinOnly**需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。<br />-   **full**會提供使用者的完整存取權，包括將電腦加入網域的許可權。|
|[/JoinDomain： {Yes &#124; No}]|指定是否應該在作業系統安裝期間，將電腦加入網域做為此電腦帳戶。 預設值為 **[是]** 。|
|[/ReferralServer：<Server name>]|指定要連接的伺服器名稱，以使用簡單的檔案傳輸通訊協定（tftp）下載網路開機程式和開機映射。|
|[/BootProgram：<Relative path>]|指定從 remoteInstall 資料夾到這部電腦應接收之網路開機程式的相對路徑。 例如： **boot\x86\pxeboot.com**。|
|[/WdsClientUnattend：<Relative path>]|指定從 remoteInstall 資料夾到自動執行 Windows 部署服務用戶端的自動安裝檔案的相對路徑。|
|[/BootImagepath：<Relative path>]|指定從 remoteInstall 資料夾到這部電腦應接收之開機映射的相對路徑。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要核准 RequestId 為12的電腦，請輸入：
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
若要核准 RequestID 為20的電腦，並使用指定的設定來部署映射，請輸入：
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:OU=Test,CN=company,DC=Domain,DC=Com /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
若要核准所有擱置中的電腦，請輸入：
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
使用[AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
使用[AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)，
[使用 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
