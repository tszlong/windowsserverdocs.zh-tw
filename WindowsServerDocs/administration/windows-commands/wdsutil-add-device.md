---
title: wdsutil 新增裝置
description: Wdsutil [新增裝置] 命令的參考文章，會在 Active Directory Domain Services 中預先設置電腦。
ms.topic: reference
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: de05d5510e61f5cba0813a7e11215935fd380968
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524634"
---
# <a name="wdsutil-add-device"></a>wdsutil 新增裝置

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory Domain Services (AD DS) 中預先設置電腦。 預先暫存的電腦也稱為 *已知電腦*。 這可讓您設定屬性來控制用戶端的安裝。 例如，您可以設定網路開機程式和用戶端應接收的自動安裝檔案，以及用戶端應從哪個伺服器下載網路開機程式。

## <a name="syntax"></a>語法

```
wdsutil /add-Device /Device:<Devicename> /ID:<UUID | MAC address> [/ReferralServer:<Servername>] [/BootProgram:<Relativepath>] [/WdsClientUnattend:<Relativepath>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relativepath>] [/OU:<DN of OU>] [/Domain:<Domain>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 裝置`<Devicename>` | 指定要新增之裝置的名稱。 |
| 識別碼`<UUID|MAC address>` | 指定電腦的 GUID/UUID 或 MAC 位址。 GUID/UUID 必須是下列兩種格式之一：二進位字串 (`/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6`) 或 guid 字串 (`/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6`) 。 MAC 位址的格式必須是下列格式： **00B056882FDC** (沒有連字號) 或 **00-B0-56-88-94-DC** (加上連字號)  |
| [/ReferralServer： `<Servername>` ] | 指定要聯繫的伺服器名稱，以使用一般檔案傳輸通訊協定 (tftp) 來下載網路開機程式和開機映射。 |
| [/BootProgram： `<Relativepath>` ] | 指定來自 **remoteInstall** 資料夾的相對路徑，到這部電腦應接收的網路開機程式。 例如：`boot\x86\pxeboot.com` |
| [/WdsClientUnattend： `<Relativepath>` ] | 將 **remoteInstall** 資料夾的相對路徑指定為自動安裝檔案，以自動安裝 Windows 部署服務用戶端的安裝畫面。 |
| [/User： `<Domain\User|User@Domain>` ] | 設定電腦帳戶物件的許可權，將電腦加入網域的必要許可權授與指定的使用者。 |
| [/JoinRights： `{JoinOnly|Full}` ] | 指定要指派給使用者的許可權類型。<ul><li>**JoinOnly** -要求系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。</li><li>**Full** -提供使用者的完整存取權，包括將電腦加入網域的許可權。 |
| [/JoinDomain： `{Yes|No}` ] | 指定是否應在作業系統安裝期間，將電腦加入網域作為此電腦帳戶。 預設值為 **[是]**。 |
| [/BootImagepath： `<Relativepath>` ] | 指定來自 **remoteInstall** 資料夾與這部電腦應使用之開機映射的相對路徑。 |
| [/OU： `<DN of OU>` ] | 應在其中建立電腦帳戶物件之組織單位的分辨名稱。 例如： **OU = MyOU，CN = Test，dc = Domain，dc = com**。 預設位置是預設電腦的容器。 |
| [/Domain： `<Domain>` ] | 應該在其中建立電腦帳戶物件的網域。 預設位置是本機網域。 |

## <a name="examples"></a>範例

若要使用 MAC 位址新增電腦，請輸入：

```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```

若要使用 GUID 字串新增電腦，請輸入：

```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil get-alldevices 命令](wdsutil-get-alldevices.md)

- [wdsutil 取得裝置命令](wdsutil-get-device.md)

- [wdsutil 設定裝置命令](wdsutil-set-device.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)

- [新的-WdsClient](/powershell/module/wds/New-WdsClient)
