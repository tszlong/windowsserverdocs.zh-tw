---
title: 新增裝置
description: 將預先設置 active directory 網域服務中電腦的新增裝置參考文章。 預先設置的電腦也稱為已知電腦。
ms.topic: reference
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5ba0d614f5212702842ac6c95eae1945f66faa05
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638317"
---
# <a name="add-device"></a>新增裝置

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預先設置 active directory 網域服務中的電腦。 預先設置的電腦也稱為已知電腦。 這可讓您設定屬性來控制用戶端的安裝。 例如，您可以設定網路開機程式和用戶端應接收的自動安裝檔案，以及用戶端應從哪個伺服器下載網路開機程式。

## <a name="syntax"></a>語法
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>]
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|裝置<computer name>|指定要新增之電腦的名稱。|
|/ID： <UUID &#124; MAC 位址>|指定電腦的 GUID/UUID 或 MAC 位址。 GUID/UUID 的格式必須是二進位字串或 GUID 字串的其中一種。 例如：<p>二進位字串： **/id： ACEFA3E81F20694E953EB2DAA1E8B1B6**<p>GUID 字串： **/id： E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<p>MAC 位址的格式必須是下列格式： **00B056882FDC** (沒有連字號) 或 **00-B0-56-88-94-DC** (加上連字號) |
|[/ReferralServer： <Server name> ]|指定要聯繫的伺服器名稱，以使用一般檔案傳輸通訊協定 (tftp) 來下載網路開機程式和開機映射。|
|[/BootProgram： <Relative path> ]|指定來自 remoteInstall 資料夾的相對路徑，到這部電腦應接收的網路開機程式。 例如： boot\x86\pxeboot.com|
|[/WdsClientUnattend： <Relative path> ]|將 remoteInstall 資料夾的相對路徑指定為自動安裝檔案，以自動安裝 Windows 部署服務用戶端的安裝畫面。|
|[/User： <Domain\User &#124; User@Domain>]|設定電腦帳戶物件的許可權，將電腦加入網域的必要許可權授與指定的使用者。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要指派給使用者的許可權類型。<p>-   **JoinOnly** 需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。<br />-   **Full** 提供使用者的完整存取權，包括將電腦加入網域的許可權。|
|[/JoinDomain： {Yes &#124; No}]|指定是否應在作業系統安裝期間，將電腦加入網域作為此電腦帳戶。 預設值為 **[是]**。|
|[/BootImagepath： <Relative path> ]|指定來自 remoteInstall 資料夾與這部電腦應使用之開機映射的相對路徑。|
|[/OU： <DN of OU> ]|應在其中建立電腦帳戶物件之組織單位的分辨名稱。 例如： **OU = MyOU，CN = Test，dc = Domain，dc = com**。 預設位置是預設電腦的容器。|
|[/Domain： <Domain> ]|應該在其中建立電腦帳戶物件的網域。 預設位置是本機網域。|
## <a name="examples"></a>範例
若要使用 MAC 位址新增電腦，請輸入：
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
若要使用 GUID 字串新增電腦，請輸入：
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AllDevices 命令](using-the-get-alldevices-command.md) 
[使用取得裝置命令](using-the-get-device-command.md) 
[子命令：設定裝置](subcommand-set-device.md) 
[新的-WdsClient](/previous-versions/windows/powershell-scripting/dn283430(v=wps.630))
