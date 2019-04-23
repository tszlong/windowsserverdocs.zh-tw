---
title: 使用 [新增裝置] 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9ef4445b4dabbe85c2397d62b06756e17879d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878539"
---
# <a name="using-the-add-device-command"></a>使用 [新增裝置] 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

預先設置在 active directory 網域服務中的電腦。 預先設置的電腦也稱為已知的電腦。 這可讓您設定屬性來控制用戶端的安裝。 例如，您可以設定網路開機程式和用戶端應接收的自動安裝檔案，以及用戶端應該下載的網路開機程式的伺服器。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
## <a name="syntax"></a>語法
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/ 裝置：<computer name>|指定要新增之電腦的名稱。|
|/ID:<UUID &#124; MAC address>|指定 GUID/UUID 或電腦的 MAC 位址。 兩種格式的二進位字串或 GUID 字串，其中之一必須是 GUID/UUID。 例如: <br /><br />二進位字串： **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br /><br />GUID 字串： **/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br /><br />MAC 位址必須是下列格式：**00B056882FDC** （沒有連字號） 或**00-B0-56-88-2F-DC** （含連接號）|
|[/ReferralServer:<Server name>]|指定要連線來使用簡單式檔案傳輸通訊協定 (tftp) 下載的網路開機程式和開機映像的伺服器名稱。|
|[/BootProgram:<Relative path>]|指定應該會收到這台電腦的網路開機程式從 remoteInstall 資料夾的相對路徑。 比方說: 「 boot\x86\pxeboot.com"|
|[/WdsClientUnattend:<Relative path>]|指定自動的安裝檔案，用來自動化 Windows 部署服務用戶端的安裝畫面從 remoteInstall 資料夾的相對路徑。|
|[/ 使用者： < 網域 \ 使用者&#124; User@Domain>]|若要將電腦加入網域的必要權限授與指定的使用者的電腦帳戶物件上設定權限。|
|[/ JoinRights: {JoinOnly&#124;完整}]|指定要指派給使用者的權限的類型。<br /><br />-   **JoinOnly**需要系統管理員使用者可以將電腦加入網域之前，重設電腦帳戶。<br />-   **完整**可完整存取權提供給使用者，其中包含將電腦加入網域的權限。|
|[/JoinDomain:{Yes &#124; No}]|指定在電腦應加入網域成為這個電腦帳戶在作業系統安裝期間。 預設值是**是**。|
|[/BootImagepath:<Relative path>]|指定應該使用這台電腦的開機映像從 remoteInstall 資料夾的相對路徑。|
|[/ OU:<DN of OU>]|建立電腦帳戶物件的所在的組織單位辨別的名稱。 例如: **OU = MyOU，CN = 測試，DC = Domain，DC = com**。 預設位置是預設電腦容器。|
|[/ 網域：<Domain>]|建立電腦帳戶物件的所在網域。 預設位置是本機網域。|
## <a name="BKMK_examples"></a>範例
若要使用的 MAC 位址，以加入電腦，請輸入：
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
若要使用為 GUID 字串，以加入電腦，請輸入：
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com 
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:"OU=MyOU,CN=Test,DC=Domain,DC=com"
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllDevices 命令](using-the-get-alldevices-command.md)
[使用 get 裝置命令](using-the-get-device-command.md)
[子命令：設定裝置](subcommand-set-device.md)
[新增 WdsClient](https://technet.microsoft.com/library/dn283430.aspx)
