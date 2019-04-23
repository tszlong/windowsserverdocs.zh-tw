---
title: 子命令設定裝置
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80bb9144936cf493784603bcbdb8a0d1e5c870bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848059"
---
# <a name="subcommand-set-device"></a>子命令： 設定裝置

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更預先設置電腦的屬性。 預先設置的電腦是已連結到 active directory (AD DS) 的網域伺服器的電腦帳戶物件的電腦。 預先設置的用戶端也稱為已知的電腦。 您可以控制用戶端安裝的電腦帳戶上設定屬性。 例如，您可以設定網路開機程式和用戶端應接收的自動安裝檔案，以及用戶端應該下載的網路開機程式的伺服器。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/ 裝置：<computer name>|指定電腦 (SAM n t-n) 的名稱。|
|[/ID:<UUID &#124; MAC address>]|指定 GUID/UUID 或電腦的 MAC 位址。 此值必須是下列三種格式之一：<br /><br />-二進位字串： **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID 字串: / 識別碼：**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-MAC 位址：**00B056882FDC** （沒有連字號） 或**00-B0-56-88-2F-DC** （含連接號）|
|[/ReferralServer:<Server name>]|指定要下載的網路開機程式和開機映像使用簡單式檔案傳輸通訊協定 (tftp) 連絡的伺服器名稱。|
|[/BootProgram:<Relative path>]|指定將會收到指定的電腦的網路開機程式從 remoteInstall 資料夾的相對路徑。 例如： **boot\x86\pxeboot.com**|
|[/WdsClientUnattend:<Relative path>]|指定自動安裝檔案，用來自動化 Windows 部署服務用戶端的安裝畫面從 remoteInstall 資料夾的相對路徑。|
|[/ 使用者： < 網域 \ 使用者&#124; User@Domain>]|若要將電腦加入網域的必要權限授與指定的使用者的電腦帳戶物件上設定權限。|
|[/ JoinRights: {JoinOnly&#124;完整}]|指定要指派給使用者的權限的類型。<br /><br />-   **JoinOnly**需要系統管理員使用者可以將電腦加入網域之前，重設電腦帳戶。<br />-   **完整**可完整存取權提供給使用者，包括將電腦加入網域的權限。|
|[/JoinDomain:{Yes &#124; No}]|指定電腦應加入網域成為這個電腦帳戶在 Windows 部署服務安裝期間。 預設值是**是**。|
|[/BootImagepath:<Relative path>]|指定電腦將使用的開機映像從 remoteInstall 資料夾的相對路徑。|
|[/ 網域：<Domain>]|指定要搜尋預先設置的電腦的網域。 預設值是本機網域。|
|[/resetAccount]|重新設定指定的電腦上的權限，讓具有適當的權限的任何人都可以使用此帳戶來加入網域。|
## <a name="BKMK_examples"></a>範例
若要設定網路開機程式，並且轉介伺服器電腦，請輸入：
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
若要設定電腦的各種設定，請輸入：
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增裝置命令](using-the-add-device-command.md)
[使用 get AllDevices 命令](using-the-get-alldevices-command.md)
[使用取得裝置命令](using-the-get-device-command.md)
