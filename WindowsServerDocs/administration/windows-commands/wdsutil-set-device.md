---
title: wdsutil 設定-裝置
description: Wdsutil 設定裝置的參考文章，變更預先設置電腦的屬性。
ms.topic: reference
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9844df011a01419599463a26cc4b78761af7cdbd
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729860"
---
# <a name="wdsutil-set-device"></a>wdsutil 設定-裝置

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更預先設置電腦的屬性。 預先設置的電腦是指已連結至 active directory 網域伺服器中電腦帳戶物件 (AD DS) 的電腦。 預先設置的用戶端也稱為已知電腦。 您可以設定電腦帳戶上的屬性，以控制用戶端的安裝。 例如，您可以設定網路開機程式和用戶端應接收的自動安裝檔案，以及用戶端應從哪個伺服器下載網路開機程式。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>]
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|裝置<computer name>|指定 (SAM 帳戶名稱) 的電腦名稱稱。|
|[/ID： <UUID &#124; MAC 位址>]|指定電腦的 GUID/UUID 或 MAC 位址。 此值必須是下列三種格式的其中一種：<p>-二進位字串： **/id： ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID 字串：/ID：**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-MAC 位址： **00B056882FDC** (沒有連字號) 或 **00-B0-56-88-2f-DC** (加上連字號) |
|[/ReferralServer： <Server name> ]|指定要聯繫的伺服器名稱，以使用一般檔案傳輸通訊協定 (tftp) 來下載網路開機程式和開機映射。|
|[/BootProgram： <Relative path> ]|指定從 remoteInstall 資料夾到指定電腦將接收的網路開機程式的相對路徑。 例如： **boot\x86\pxeboot.com**|
|[/WdsClientUnattend： <Relative path> ]|指定從 remoteInstall 資料夾到自動安裝檔案的相對路徑，此檔案會自動執行 Windows 部署服務用戶端的安裝畫面。|
|[/User： <Domain\User &#124; User@Domain>]|設定電腦帳戶物件的許可權，將電腦加入網域的必要許可權授與指定的使用者。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要指派給使用者的許可權類型。<p>-   **JoinOnly** 需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。<br />-   **Full** 提供使用者的完整存取權，包括將電腦加入網域的許可權。|
|[/JoinDomain： {Yes &#124; No}]|指定是否應該在 Windows 部署服務安裝期間，將電腦加入網域作為此電腦帳戶。 預設值為 [是]。|
|[/BootImagepath： <Relative path> ]|指定從 remoteInstall 資料夾到電腦將使用之開機映射的相對路徑。|
|[/Domain： <Domain> ]|指定要搜尋預先設置電腦的網域。 預設值為本機網域。|
|[/resetAccount]|重設指定電腦上的許可權，讓具有適當許可權的任何人都可以使用此帳戶加入網域。|
## <a name="examples"></a>範例
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
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil 新增裝置命令](wdsutil-add-device.md)
- [wdsutil get-alldevices 命令](wdsutil-get-alldevices.md)
- [wdsutil 取得裝置命令](wdsutil-get-device.md)
