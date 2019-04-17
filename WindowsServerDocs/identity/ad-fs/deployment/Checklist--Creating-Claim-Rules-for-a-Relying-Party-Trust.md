---
ms.assetid: 44271f44-b50a-4bce-9375-4fcab9618048
title: "信任的檢查清單-信賴建立理賠要求規則"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9c75cd4ccbafefdda83cba4551fd6b9af63c4822
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-creating-claim-rules-for-a-relying-party-trust"></a>檢查清單︰ 建立信賴的派對信任理賠要求規則

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檢查此清單會包括所需的規劃、設計，和部署理賠要求規則信賴廠商信任 Active Directory 同盟服務 \(AD FS\) 中相關聯的工作。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的程序時，返回本主題之後在您完成該程序中的步驟操作，以便您可以繼續檢查清單中的其餘的工作。  
  
![建立取得規則](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 設定信賴的派對信任建立理賠要求規則**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![建立理賠要求規則](media/icon_checkboxo.gif)|檢視宣告概念、取得規則、取得規則集，並取得規則範本和方式的相關聯聯盟信任。|![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[宣告角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[的角色的取得規則](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![建立理賠要求規則](media/icon_checkboxo.gif)|檢視概念有關如何理賠要求經由宣告發行管線中的所有階段和規則的處理方式主張發行引擎。|![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[宣告管線的角色](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[宣告引擎的角色](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![建立理賠要求規則](media/icon_checkboxo.gif)|有效規劃和實作發出透過這個信賴廠商信任輸出宣告，判斷是否需要一或多個理賠要求規則及的取得您應該使用此可以廠商信任規則。|![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷類型的取得規則範本使用](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![建立理賠要求規則](media/icon_checkboxo.gif)|建立一個宣告規則透過其他和您如何使用為了提供您想要的結果理想的輸出中提供更複雜的邏輯比標準規則理賠要求規則語言取得設定時，請檢查有關的概念。|![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用傳遞透過或篩選取得規則](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用轉換取得規則](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用傳送 LDAP 屬性宣告規則](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[以取得規則使用傳送群組成員資格](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用授權取得規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用自訂取得規則](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![建立取得規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[取得規則語言的角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![建立理賠要求規則](media/icon_checkboxo.gif)|如果不存在，必須先建立宣告描述，將會滿足您組織的需求。 AD FS 隨附宣告描述公開 AD FS 管理 snap\ 中的預設設定。|![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[需要新增描述宣告](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![建立理賠要求規則](media/icon_checkboxo.gif)|根據您的組織的需求，建立的規則集合宣告將會在適當發行這個信賴廠商信任相關聯的一或多個理賠要求規則。|![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立傳遞透過規則或篩選輸入宣告](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立為宣告傳送 LDAP 屬性規則](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立傳送群組成員資格為理賠要求規則](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則轉換輸入宣告](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立傳送驗證方法取得規則](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立傳送給 AD FS 規則 1.x 相容宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立自訂規則傳送主張使用規則](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
|![建立理賠要求規則](media/icon_checkboxo.gif)|根據您的組織的需求，建立發行授權規則設定或委派授權規則集合，讓使用者可以存取信賴這個信賴廠商信任相關聯的一或多個理賠要求規則。|![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立允許所有使用者規則](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)<br /><br />![建立取得規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[上連入取得建立規則允許或根據拒絕使用者](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)|  
