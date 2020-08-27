---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory 整個網域的架構更新
description: 升級網域控制站時，由 adprep/domainprep 執行的全網域架構更新
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.openlocfilehash: cc22ebec3546c852dff811b00a0e430c7f740abc
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88940778"
---
# <a name="domain-wide-schema-updates"></a>全網域架構更新

>適用於：Windows Server

您可以檢查下列變更集，以協助瞭解並準備 Windows Server 中的 adprep/domainprep 所執行的架構更新。

從 Windows Server 2012 開始，Adprep 命令會在 AD DS 安裝期間視需要自動執行。 它們也可以在 AD DS 安裝之前分開執行。 如需詳細資訊，請參閱[執行 Adprep.exe](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10))。

如需有關如何解讀 (ACE) 字串之存取控制專案的詳細資訊，請參閱 [ace 字串](/windows/win32/secauthz/ace-strings)。 如需有關如何將安全識別碼解讀 (SID) 字串的詳細資訊，請參閱 [sid 字串](/windows/win32/secauthz/sid-strings)。

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (半年通道) ：全網域更新

**在 Windows Server 2016 中的**「完成」 (操作 89) 完成的作業之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 物件的**修訂**屬性會設定為**16**。

|作業編號和 GUID|描述|權限|
|------------------------------|---------------|--------------|---------------|
|**操作 89**： {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|刪除 ACE 授與 Enterprise Key Admins 的完整控制權，並新增 ACE 授與 Enterprise Key Admins 的完整控制權僅限 msdsKeyCredentialLink 屬性。|刪除 (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員)  <br /> <br />新增 (的 OA;CIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;企業金鑰管理員) |

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016：全網域更新

**在 Windows Server 2016 中的**「完成」 (操作 82-88) 完成的作業之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 物件的**修訂**屬性會設定為**15**。

|作業編號和 GUID|描述|屬性|權限|
|------------------------------|---------------|--------------|---------------|
|**操作 82**： {83C53DA7-427E-47A4-A07A-A324598B88F7}|在網域根目錄建立 CN = Keys 容器|-objectClass：容器<br />-description： key credential 物件的預設容器<br />-ShowInAdvancedViewOnly： TRUE| (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;EA) <br /> (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;D) <br /> (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;SY) <br /> (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;DD) <br /> (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED) |
|**操作 83**： {C81FC9CC-0130-4FD1-B272-634D74818133}|將 [完全控制] 允許 ace 設為 "domain\Key Admins" 和 "rootdomain\Enterprise Key Admins" 的 CN = Keys 容器。|N/A| (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;金鑰管理員) <br /> (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員) |
|**操作 84**： {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|修改 otherWellKnownObjects 屬性以指向 CN = Keys 容器。|-otherWellKnownObjects： B:32：683A24E2E8164BD3AF86AC3C2CF3F981： CN = Keys，% ws|N/A|
|**操作 85**： {e6d5fd00-385d-4e65-b02d-9da3493ed850}|修改網域 NC 以允許 "domain\Key Admins" 和 "rootdomain\Enterprise Key Admins" 修改 Msds-keycredentiallink 屬性。 |N/A| (的 OA;CIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;金鑰管理員) <br /> (的 OA;CIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;根域中的企業金鑰系統管理員，但在非根域中，會產生具有不可解析-527 SID 的假網域相關 ACE) |
|**操作 86**： {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|將 DS 驗證-寫入電腦的車輛授與 creator owner 和 self|N/A| (的 OA;CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-a285-00aa003049e2; PS) <br /> (的 OA;CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-a285-00aa003049e2; CO) |
|**操作 87**： {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|刪除 ACE 授與不正確的網域相對企業金鑰系統管理員群組的完整控制權，並新增 ACE，授與 Enterprise Key Admins 群組的完全控制許可權。 |N/A|刪除 (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員) <br /> <br />新增 (A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企業金鑰管理員) |
|**操作 88**： {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|在網域 NC 物件上新增 "ExpirePasswordsOnSmartCardOnlyAccounts"，並將預設值設定為 FALSE|N/A|N/A|

Enterprise Key Admins 和 Key Admins 群組只會在升級 Windows Server 2016 網域控制站之後建立，並接管 PDC 模擬器 FSMO 角色。

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2：全網域更新

雖然在 Windows Server 2012 R2 中的「 **域** 控制器」完成作業並不會執行任何作業，但是在命令完成之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 物件的 **修訂** 屬性會設定為 **10**。

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012：全網域更新

**在 Windows Server 2012 中的**「完成」 (操作78、79、80和 81) 完成之前執行的作業之後，Cn = ACTIVEDIRECTORYUPDATE，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 物件的**修訂**屬性會設定為**9**。

|作業編號和 GUID|描述|屬性|權限|
|------------------------------|---------------|--------------|---------------|
|**操作 78**： {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|在網域分割區中建立新的物件 CN = TPM 裝置。|物件類別： Mstpm-ownerinformation-InformationObjectsContainer|N/A|
|**操作 79**： {54afcfb9-637a-4251-9f47-4d50e7021211}|已建立 TPM 服務的存取控制專案。|N/A| (的 OA;CIIO;WP; ea1b7b93-5e48-46d5-bc6c-4df4fda78a35; bf967a86-0de6-11d0-a285-00aa003049e2; PS) |
|**操作 80**： {f4728883-84dd-483c-9897-274f2ebcf11e}|將「複製 DC」擴充許可權授與 **Cloneable 網域控制站** 群組|N/A| (的 OA;;CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;;*網域 SID*-522) |
|**操作 81**： {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|在所有物件上，將允許的其他使用者身分識別授與給主體本身。|N/A| (的 OA;CIOI;RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;PS) |
