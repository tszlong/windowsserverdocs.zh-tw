---
title: 子命令組-裝置
description: 適用于子命令集的 Windows 命令主題-裝置會變更預先設置之電腦的屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ced5f44c9edda0176c93479ded60d809644bf4fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834041"
---
# <a name="subcommand-set-device"></a>子命令：設定-裝置

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更預先設置之電腦的屬性。 預先設置的電腦是指已連結到 active directory 網域伺服器（AD DS）中電腦帳戶物件的電腦。 預先設置的用戶端也稱為已知的電腦。 您可以設定電腦帳戶的屬性，以控制用戶端的安裝。 例如，您可以設定網路開機程式和用戶端應接收的自動安裝檔案，以及用戶端應該從中下載網路開機程式的伺服器。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/Device：<computer name>|指定電腦的名稱（SAM 帳戶名稱）。|
|[/ID： < UUID &#124; MAC 位址 >]|指定電腦的 GUID/UUID 或 MAC 位址。 此值必須是下列三種格式的其中一種：<p>-二進位字串： **/id： ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID 字串：/ID：**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-MAC 位址： **00B056882FDC** （無破折號）或**00-B0-56-88-2f-** （含破折號）|
|[/ReferralServer：<Server name>]|指定要連線的伺服器名稱，以使用簡單的檔案傳輸通訊協定（tftp）下載網路開機程式和開機映射。|
|[/BootProgram：<Relative path>]|指定從 remoteInstall 資料夾到指定電腦將接收之網路開機程式的相對路徑。 例如： **boot\x86\pxeboot.com**|
|[/WdsClientUnattend：<Relative path>]|指定從 remoteInstall 資料夾到自動安裝檔案的相對路徑，該檔案會將 Windows 部署服務用戶端的安裝畫面自動化。|
|[/User： < Domain\User &#124; User@Domain>]|設定電腦帳戶物件的許可權，授與指定的使用者將電腦加入網域所需的許可權。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要指派給使用者的許可權類型。<p>-   **JoinOnly**需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。<br />-   **full**會提供使用者的完整存取權，包括將電腦加入網域的許可權。|
|[/JoinDomain： {Yes &#124; No}]|指定在 Windows 部署服務安裝期間，是否應將電腦加入網域做為此電腦帳戶。 預設設定為 **[是]** 。|
|[/BootImagepath：<Relative path>]|指定從 remoteInstall 資料夾到電腦將使用之開機映射的相對路徑。|
|[/Domain：<Domain>]|指定要搜尋預先設置電腦的網域。 預設值為本機網域。|
|[/resetAccount]|重設指定電腦上的許可權，讓具有適當許可權的任何人都可以使用此帳戶來加入網域。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要設定電腦的網路開機程式和參照伺服器，請輸入：
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
若要設定電腦的各種設定，請輸入：
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
[使用 [新增裝置] 命令](using-the-add-device-command.md)
使用 [[取得-裝置](using-the-get-device-command.md)] 命令
[的 AllDevices 命令](using-the-get-alldevices-command.md)
