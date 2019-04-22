---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory 的全網域的結構描述更新
description: 執行 adprep /domainprep 時升級網域控制站的全網域的結構描述更新
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 20598431b9c2376051247fd7ba14e33af00968cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812319"
---
# <a name="domain-wide-schema-updates"></a>全網域的結構描述更新

>適用於：Windows Server

您可以檢閱下列一組變更以協助了解及準備執行 adprep /domainprep，Windows Server 中的結構描述更新。

Adprep 命令從 Windows Server 2012 中，視需要在 AD DS 安裝期間自動執行。 它們也可以執行個別的 AD DS 安裝之前。 如需詳細資訊，請參閱[執行 Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx)。

如需如何解譯存取控制項目 (ACE) 字串的詳細資訊，請參閱[ACE 字串](https://msdn.microsoft.com/library/aa374928(VS.85).aspx)。 如需如何解譯的安全性識別碼 (SID) 字串的詳細資訊，請參閱[SID 字串](https://msdn.microsoft.com/library/aa379602(VS.85).aspx)。

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server （半年通道）：全網域更新

所執行的作業之後**domainprep** Windows Server 2016 中 （作業 89） 完成時，**修訂**屬性 CN = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC =ForestRootDomain 物件設定為**16**。

|作業數目和 GUID|描述|Permissions|
|------------------------------|---------------|--------------|---------------|
|**作業 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|刪除 ACE 授予完全控制給 Enterprise Key Admins，然後新增 ACE 授與 Enterprise Key Admins 全權掌控 msdsKeyCredentialLink 屬性。|刪除 (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業重要的系統管理員） <br /> <br />新增 (OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;企業重要的系統管理員）|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016:全網域更新

所執行的作業之後**domainprep** Windows Server 2016 中 （作業 82 88） 完成時，**修訂**屬性 CN = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC = 的 ForestRootDomain 物件設定為**15**。

|作業數目和 GUID|描述|屬性|Permissions|
|------------------------------|---------------|--------------|---------------|
|**作業 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|建立 CN = 網域根目錄的金鑰容器|-objectClass： 容器<br />-描述：金鑰認證物件的預設容器<br />-ShowInAdvancedViewOnly:TRUE|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;EA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;SY)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DD)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED)|
|**作業 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|完整控制權新增允許 ace 至 CN ="domain\Key 系統管理員 」 的金鑰容器和 「 rootdomain\Enterprise Key Admins"。|N/A|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;索引鍵的系統管理員）<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業重要的系統管理員）|
|**作業 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|修改另一個 Wellknownobjects 屬性，指向 CN = 金鑰容器。|-另一個 Wellknownobjects:B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN=Keys,%ws|N/A|
|**作業 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|修改網域 NC，以允許 「 domain\Key 系統管理員 」 和 「 rootdomain\Enterprise Key Admins 」 修改 Msds-keycredentiallink 屬性。 |N/A|(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;索引鍵的系統管理員）<br />(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;在根網域，但非根網域中 Enterprise Key Admins 導致無法解析的-527 sid 假的網域相對 ACE）|
|**作業 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|DS-驗證-寫入-電腦汽車授與建立者擁有者及自我|N/A|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**作業 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|刪除 ACE 授予完全控制給不正確的網域相對 Enterprise Key Admins 群組，然後加入 ACE 授予完全控制給 Enterprise Key Admins 群組。 |N/A|刪除 (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業重要的系統管理員）<br /> <br />將 (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企業重要的系統管理員）|
|**作業 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|新增網域 NC 物件上的 「 msDS ExpirePasswordsOnSmartCardOnlyAccounts"和預設值設定為 FALSE|N/A|N/A|

Windows Server 2016 網域控制站已升級，並會於 PDC 模擬器 FSMO 角色之後，才會建立 Enterprise Key Admins 和 Key Admins 群組。

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2：全網域更新

雖然任何作業都不由**domainprep** Windows Server 2012 R2 中，在命令完成之後,**修訂**屬性 CN = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC = 的 ForestRootDomain 物件設定為**10**。

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012:全網域更新

所執行的作業之後**domainprep**在 Windows Server 2012 (operations 78、 79、 80 和 81) 完成時，**修訂**屬性 CN = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC = 的 ForestRootDomain 物件設定為**9**。

|作業數目和 GUID|描述|屬性|Permissions|
|------------------------------|---------------|--------------|---------------|
|**作業 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|建立新的物件 CN = 網域分割中的 TPM 裝置。|物件類別： msTPM InformationObjectsContainer|N/A|
|**作業 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|建立 TPM 服務的存取控制項目。|N/A|(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**作業 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|授與 「 複製 DC 」 擴充權限**Cloneable Domain Controllers**群組|N/A|(OA;CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;*網域 SID*-522)|
|**作業 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|授與 ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity 原則本身上的所有物件。|N/A|(OA;CIOI;RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;PS)|
