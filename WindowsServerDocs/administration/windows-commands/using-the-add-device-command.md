---
title: 新增裝置
description: 新增裝置的參考文章，它會在 active directory 網域服務中將預先設置電腦。 預先設置的電腦也稱為已知的電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e410132ea3d7ce151c47d4708f284a8e44448aaf
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937242"
---
# <a name="add-device"></a>新增裝置

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 active directory 網域服務中將預先設置電腦。 預先設置的電腦也稱為已知的電腦。 這可讓您設定屬性來控制用戶端的安裝。 例如，您可以設定網路開機程式和用戶端應接收的自動安裝檔案，以及用戶端應該從中下載網路開機程式的伺服器。

## <a name="syntax"></a>語法
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>]
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|主設備<computer name>|指定要新增之電腦的名稱。|
|/ID： <UUID &#124; MAC 位址>|指定電腦的 GUID/UUID 或 MAC 位址。 GUID/UUID 必須是兩種格式的其中一種：二進位字串或 GUID 字串。 例如：<p>二進位字串： **/id： ACEFA3E81F20694E953EB2DAA1E8B1B6**<p>GUID 字串： **/id： E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<p>MAC 位址的格式必須如下： **00B056882FDC** （無連字號）或**00-B0-56-88-2f-DC** （含破折號）|
|[/ReferralServer： <Server name> ]|指定要連接的伺服器名稱，以使用簡單的檔案傳輸通訊協定（tftp）下載網路開機程式和開機映射。|
|[/BootProgram： <Relative path> ]|指定從 remoteInstall 資料夾到這部電腦應接收之網路開機程式的相對路徑。 例如： boot\x86\pxeboot.com|
|[/WdsClientUnattend： <Relative path> ]|指定從 remoteInstall 資料夾到自動安裝檔案的相對路徑，該檔案會將 Windows 部署服務用戶端的安裝畫面自動化。|
|[/User： <Domain\User &#124; User@Domain>]|設定電腦帳戶物件的許可權，授與指定的使用者將電腦加入網域所需的許可權。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要指派給使用者的許可權類型。<p>-   **JoinOnly**需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。<br />-   **Full**會提供使用者的完整存取權，包括將電腦加入網域的許可權。|
|[/JoinDomain： {Yes &#124; No}]|指定是否應該在作業系統安裝期間，將電腦加入網域做為此電腦帳戶。 預設值為 **[是]**。|
|[/BootImagepath： <Relative path> ]|指定從 remoteInstall 資料夾到這部電腦應使用之開機映射的相對路徑。|
|[/OU： <DN of OU> ]|應在其中建立電腦帳戶物件之組織單位的辨別名稱。 例如： **OU = MyOU，CN = Test，dc = Domain，DC = com**。 預設位置為預設電腦的容器。|
|[/Domain： <Domain> ]|應在其中建立電腦帳戶物件的網域。 預設位置是本機網域。|
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
[子命令：設定-裝置](subcommand-set-device.md) 
[新的-WdsClient](https://technet.microsoft.com/library/dn283430.aspx)
