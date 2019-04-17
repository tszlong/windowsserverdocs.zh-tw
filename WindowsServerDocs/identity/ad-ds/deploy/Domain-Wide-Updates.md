---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: "Active Directory domain 全架構更新"
description: "由 adprep 全網域架構更新 /domainprep 升級網域控制站"
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 07/14/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59efb7c854b7f3693776bf9a1a371f98c2206d60
ms.sourcegitcommit: 79c1359232bece2e5c3ee5507f1e4bf19b44f487
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2018
---
# <a name="domain-wide-schema-updates"></a>全網域架構更新

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以檢視的下列設定的變更，以了解和準備由 adprep 架構更新協助 /domainprep 在 Windows Server 中。 

Adprep 命令開始在 Windows Server 2012，視 AD DS 安裝期間自動執行。 它們也可以用分開之前 AD DS 安裝。 如需詳細資訊，請查看[執行 Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx)。

如需如何解譯存取控制項目 (A) 字串，請查看[字串 a](https://msdn.microsoft.com/library/aa374928(VS.85).aspx)。 如需如何解譯安全性 ID (SID) 字串，請查看[字串 SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx)。

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server（以每年次通道）：全網域更新

後所執行作業**準備網域**在 Windows Server 2016（操作 89）完成，**修訂**屬性 DATA-CN = ActiveDirectoryUpdate，DATA-CN = DomainUpdates，DATA-CN = 系統特區 = ForestRootDomain 物件為**16**。

|作業數字和 GUID|描述|權限|
|------------------------------|---------------|--------------|---------------|
|**操作 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Delete a 完全控制授與企業鍵系統管理員」，並加入 a 授與企業鍵管理員完整控制權 msdsKeyCredentialLink 屬性。|Delete (A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業金鑰系統管理員」） <br /> <br />[新增 (OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;企業金鑰系統管理員」）|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016：全網域更新

後所執行作業**準備網域**在 Windows Server 2016（作業 82-88）完成，**修訂**屬性 DATA-CN = ActiveDirectoryUpdate，DATA-CN = DomainUpdates，DATA-CN = 系統特區 = 的 ForestRootDomain 物件為**15**。

|作業數字和 GUID|描述|屬性|權限|
|------------------------------|---------------|--------------|---------------|
|**操作 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|建立 DATA-CN = 金鑰容器的網域根。|-objectClass：容器<br />-描述：預設鍵認證物件的容器<br />-ShowInAdvancedViewOnly: TRUE|(A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;EA)<br />(A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;; DA)<br />(A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;SY)<br />(A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;; DD)<br />(A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;ED)|
|**操作 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|新增完全控制允許 a 到 DATA-CN = 金鑰容器的「domain\Key 系統管理員」及「rootdomain\Enterprise 鍵系統管理員」。|不適用|(A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;重要的系統管理員）<br />(A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業金鑰系統管理員」）|
|**操作 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|修改 otherWellKnownObjects 屬性，指向 [DATA-CN = 金鑰容器。|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN = 按鍵，%ws|不適用|
|**操作 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|修改網域 NC 允許」domain\Key 系統管理員」及「rootdomain\Enterprise 鍵系統管理員」修改 msds-KeyCredentialLink 屬性。 |不適用|(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;重要的系統管理員）<br />(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;企業鍵系統管理員根網域中，而非根網域導致非解析-527 sid 假網域相對 a）|
|**操作 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|建立者擁有者和自我 DS 驗證-寫入-電腦車上的授權|不適用|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**操作 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Delete a 完全控制授與的正確網域相對企業鍵系統管理員群組中，並新增 a 完全控制授與企業鍵系統管理員」群組。 |不適用|Delete (A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業金鑰系統管理員」）<br /> <br />[新增 (A。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業金鑰系統管理員」）|
|**操作 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|「MsDS ExpirePasswordsOnSmartCardOnlyAccounts」加入網域 NC 物件和預設值為 \ [false\]|不適用|不適用|

Windows Server 2016 網域控制站升級後接管 PDC 模擬器 FSMO 的角色，才會建立企業鍵系統管理員和金鑰系統管理員」群組。

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2：全網域更新

雖然不作業，**準備網域**Windows Server 2012 R2 命令後，在**修訂**屬性 DATA-CN = ActiveDirectoryUpdate，DATA-CN = DomainUpdates，DATA-CN = 系統特區 = 的 ForestRootDomain 物件為**10**。

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012：全網域更新

後所執行作業**準備網域**Windows Server 2012 中（作業 78、79、80 和 81）完成，**修訂**屬性 DATA-CN = ActiveDirectoryUpdate，DATA-CN = DomainUpdates，DATA-CN = 系統特區 = 的 ForestRootDomain 物件為**9**。

|作業數字和 GUID|描述|屬性|權限|
|------------------------------|---------------|--------------|---------------|
|**操作 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|建立新物件 DATA-CN = 網域磁碟分割中的 TPM 裝置。|物件課程：msTPM-InformationObjectsContainer|不適用|
|**操作 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|建立 TPM 服務存取控制項目。|不適用|(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**操作 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|授與延伸由右至「複製俠」**的網域控制站複製**群組|不適用|(OA;CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;*網域 SID*-522)|
|**操作 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|原則本身 MS-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity 授上所有的物件。|不適用|(OA;CIOI;RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;PS)|
