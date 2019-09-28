---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory 全網域架構更新
description: 升級網域控制站時，adprep/domainprep 所執行的全網域架構更新
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 28bb22c0f2dc70e899fe5ddfe232eacedfd400bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390764"
---
# <a name="domain-wide-schema-updates"></a>全網域架構更新

>適用於：Windows Server

您可以檢查下列變更集，以協助瞭解並準備 Windows Server 中 adprep/domainprep 所執行的架構更新。

從 Windows Server 2012 開始，在 AD DS 安裝期間，會視需要自動執行 Adprep 命令。 它們也可以在 AD DS 安裝之前分別執行。 如需詳細資訊，請參閱[執行 Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx)。

如需如何解讀存取控制專案（ACE）字串的詳細資訊，請參閱[ACE 字串](https://msdn.microsoft.com/library/aa374928(VS.85).aspx)。 如需有關如何解讀安全識別碼（SID）字串的詳細資訊，請參閱[SID 字串](https://msdn.microsoft.com/library/aa379602(VS.85).aspx)。

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server （半年通道）：全網域更新

當 Windows Server 2016 （作業 89 **）中的**「完成功能」執行的操作完成之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 物件的**修訂**屬性會設定為**16**.

|作業編號和 GUID|描述|Permissions|
|------------------------------|---------------|--------------|---------------|
|作業**89**： {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|刪除將完全控制授與企業金鑰管理員的 ACE，並新增 ACE 授與企業金鑰管理員完全控制 msdsKeyCredentialLink 屬性。|Delete （A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員） <br /> <br />新增（OA;CIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;企業金鑰管理員）|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016：全網域更新

在 Windows Server 2016 （作業 82-88 **）中的**「完成功能」執行的作業完成之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 物件的**修訂**屬性會設定為**15**.

|作業編號和 GUID|描述|屬性|Permissions|
|------------------------------|---------------|--------------|---------------|
|作業**82**： {83C53DA7-427E-47A4-A07A-A324598B88F7}|在網域根目錄建立 CN = Keys 容器|-objectClass：容器<br />描述金鑰認證物件的預設容器<br />ShowInAdvancedViewOnlyTRUE|為CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;中文<br />為CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;D為<br />為CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;SY<br />為CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;DD<br />為CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED-24000B|
|作業**83**： {C81FC9CC-0130-4FD1-B272-634D74818133}|將 [完全控制] [允許 ace] 新增至 "domain\Key Admins" 和 "rootdomain\Enterprise Key Admins" 的 CN = Keys 容器。|N/A|為CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;金鑰管理員）<br />為CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員）|
|作業**84**： {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|修改 otherWellKnownObjects 屬性以指向 CN = Keys 容器。|- otherWellKnownObjects:B:32：683A24E2E8164BD3AF86AC3C2CF3F981： CN = Keys，% ws|N/A|
|作業**85**： {e6d5fd00-385d-4e65-b02d-9da3493ed850}|修改網域 NC 以允許 "domain\Key Admins" 和 "rootdomain\Enterprise Key Admins" 修改 Msds-keycredentiallink 屬性。 |N/A|OACIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;金鑰管理員）<br />OACIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;根域中的企業金鑰管理員，但是在非根域中，產生了具有無法解析-527 SID 的偽網域相對 ACE）|
|作業**86**： {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|將 DS 驗證-寫入電腦的汽車授與 creator 擁有者和自助|N/A|OACIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-a285-00aa003049e2; PS）<br />OACIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-a285-00aa003049e2; CO）|
|作業**87**： {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|刪除將完全控制授與不正確的網域相對 Enterprise Key Admins 群組的 ACE，並新增 ACE 授與完全控制給 Enterprise Key Admins 群組。 |N/A|Delete （A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員）<br /> <br />新增（A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員）|
|作業**88**： {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|在網域 NC 物件上新增 "ExpirePasswordsOnSmartCardOnlyAccounts"，並將預設值設為 FALSE|N/A|N/A|

「企業金鑰管理員」和「金鑰管理員」群組只會在 Windows Server 2016 網域控制站升級之後建立，並接管 PDC 模擬器 FSMO 角色。

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2：全網域更新

雖然 Windows Server 2012 R2 中的 [**域**控制器] 未執行任何作業，但在命令完成之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 物件的**修訂**屬性會設定為**10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012：全網域更新

在 Windows Server 2012 （作業78、79、80和 81 **）中的**[完成] 執行的作業完成之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，CN = SYSTEM，DC = ForestRootDomain 物件的**修訂**屬性為設定為**9**。

|作業編號和 GUID|描述|屬性|Permissions|
|------------------------------|---------------|--------------|---------------|
|作業**78**： {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|在網域分割區中建立新的物件 CN = TPM 裝置。|物件類別： Mstpm-ownerinformation-InformationObjectsContainer|N/A|
|作業**79**： {54afcfb9-637a-4251-9f47-4d50e7021211}|建立 TPM 服務的存取控制專案。|N/A|OACIIO;WP; ea1b7b93-5e48-46d5-bc6c-4df4fda78a35; bf967a86-0de6-11d0-a285-00aa003049e2; PS）|
|作業**80**： {f4728883-84dd-483c-9897-274f2ebcf11e}|授與「複製 DC」延伸到**Cloneable 網域控制站**群組的許可權|N/A|（OA;;CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;;*網域 SID*-522）|
|作業**81**： {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|在所有物件上，將以 ms DS 允許的其他身分識別，授與主體本身。|N/A|OACIOI;RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;專業|
