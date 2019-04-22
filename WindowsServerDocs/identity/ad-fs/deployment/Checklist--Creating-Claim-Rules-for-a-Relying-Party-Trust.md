---
ms.assetid: 44271f44-b50a-4bce-9375-4fcab9618048
title: 檢查清單-Creating Claim Rules for 信賴憑證者信任
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9c75cd4ccbafefdda83cba4551fd6b9af63c4822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817399"
---
# <a name="checklist-creating-claim-rules-for-a-relying-party-trust"></a>檢查清單：建立信賴憑證者信任宣告規則

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

這份檢查清單包含工作所需的規劃、 設計和部署宣告規則的信賴憑證者信任中 Active Directory Federation Services 與相關聯\(AD FS\)。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![建立宣告規則](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：建立信賴憑證者信任宣告規則集**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![建立宣告規則](media/icon_checkboxo.gif)|檢閱有關宣告的概念、 宣告規則、 宣告規則集和宣告規則範本，以及它們相關聯的同盟信任。|![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of Claims](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規則的角色宣告](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![建立宣告規則](media/icon_checkboxo.gif)|檢閱有關宣告透過宣告發行管線中的所有階段流動的方式，以及如何由宣告發行引擎處理規則的概念。|![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of the Claims Pipeline](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![建立宣告規則](media/icon_checkboxo.gif)|若要有效規劃及實作會透過此信賴憑證者信任發出輸出宣告，判斷是否需要一或多個宣告規則以及哪個宣告的規則您應該使用與此信賴憑證者信任。|![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷類型的宣告規則範本使用](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![建立宣告規則](media/icon_checkboxo.gif)|若要建立一個宣告，透過另一個規則，以及如何使用宣告規則語言，以及在以提供更複雜的邏輯比標準規則，以提供所要的結果，在理想的輸出宣告集時，請檢閱相關的概念。|![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用傳遞或篩選宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用轉換宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用 a Send LDAP Attributes as Claims Rule 的時機](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用傳送的群組成員資格宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用自訂宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![建立宣告規則](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[宣告規則語言的角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![建立宣告規則](media/icon_checkboxo.gif)|如果不存在，就必須建立宣告描述的因應組織的需求。 AD FS 隨附一組預設的宣告描述會公開在 AD FS 管理嵌入式管理單元中\-中。|![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增宣告描述](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![建立宣告規則](media/icon_checkboxo.gif)|根據您組織的需求，建立會與此信賴憑證者信任相關聯，以便將適當地發出的宣告規則集的一或多個宣告規則。|![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則來傳遞或篩選傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則，以宣告方式傳送 LDAP 屬性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則，以宣告形式傳送群組成員資格](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則來轉換傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則來傳送驗證方法宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則來傳送 AD FS 1.x 相容的宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則來傳送宣告使用自訂規則](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
|![建立宣告規則](media/icon_checkboxo.gif)|根據您組織的需求，建立發行授權規則集或為此信賴憑證者信任相關聯，以便存取信賴憑證者將允許使用者委派授權規則集中的一或多個宣告規則合作對象。|![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立規則以允許所有使用者](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)<br /><br />![建立宣告規則](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[上連入宣告建立規則以允許或拒絕使用者基礎](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)|  
