---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: "判斷理賠要求規則範本使用類型"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 129cd83be4cd8302bd170ba87aad58c50f636006
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>判斷理賠要求規則範本使用類型

>適用於：Windows Server 2016、Windows Server 2012 R2

重點 Active Directory 同盟服務 \(AD FS\) 基礎結構的設計判斷一組完整的理賠要求規則-的對應取得您應該建立這些使用規則範本和 — 聯盟與您的組織會參與每個協力廠商的。 您使用理賠要求規則範本 AD FS 管理 snap\ 中建立規則。  
  
只能與一聯盟信任相關聯理賠要求規則您設定的每個設定。 這表示您無法建立一個信任一組規則，他們使用您同盟服務中其他信任。 改為您可以輕鬆地建立規則從宣告更快速地協助產生一組想在每個聯盟協力廠商與您的組織同意宣告規則範本。  
  
如需有關規則規則範本，請查看[的理賠要求規則角色](The-Role-of-Claim-Rules.md)。  
  
判斷類型理賠要求規則範本，您應該會使用您開始之前，請考慮將下列問題：  
  
-   有哪些宣告將會提供您信任的宣告提供者嗎？  
  
-   您信任的每個宣告提供者所宣告？  
  
-   哪些宣告所需的信任此同盟服務信賴派對？  
  
-   宣告哪些您願意洩露以每個信賴嗎？  
  
-   哪些使用者應該會有的存取權的每個信賴？  
  
回答這些問題可協助您計畫透明取得規則設計。 將也會協助您建立平滑授權存取控制策略和推出期間，讓您的部署小組更有效率。  
  
您可以根據您的企業環境深入了解以選取 [規則範本類型下一節需要。  
  
## <a name="claim-rule-template-types"></a>宣告規則範本類型  
下表描述所有類型的您可以建立規則使用 AD FS 管理 snap\ 中使用的優點範本輸入到另一個理賠要求規則範本。  
  
|規則範本類型|描述|優點|缺點|  
|----------------------|---------------|--------------|-----------------|  
|通過或篩選連入宣告|用來建立通過所有宣告值，選取的宣告類型或篩選依據宣告值，以便僅選取的宣告類型某些宣告值會都通過宣告規則。<br /><br />如需詳細資訊，請查看[使用傳遞透過或篩選理賠要求規則](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)。|-可以用來選取接受或發出特定宣告變更|-宣告無法變更輸入和值。|  
|轉換輸入宣告|用來建立規則，可以選取連入宣告並將它對應至不同宣告類型或地圖它理賠要求的值為新理賠要求值。<br /><br />如需詳細資訊，請查看[使用轉換理賠要求規則](When-to-Use-a-Transform-Claim-Rule.md)。|-用以標準化宣告類型或值<br />-可以取代 e\ 郵件的尾碼連入宣告|-更複雜字串更換商品需要自訂規則|  
|傳送 LDAP 屬性為宣告|用來建立將傳送至信賴宣告 LDAP 屬性市集從選取屬性規則。<br /><br />如需詳細資訊，請查看[使用傳送 LDAP 屬性宣告規則以](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)。|-從任何廣告 DS\ 日 AD LDS 屬性市集宣告可資料來源<br />-使用單一規則可以發行多宣告|效能 – slow account 搜尋結果<br />-無法使用查詢自訂 LDAP 篩選|  
|為理賠要求傳送群組成員資格|用來建立可傳送指定的宣告類型與值使用者時的 Active Directory 安全性群組成員規則。 將會使用此規則，根據您所選取的群組傳送單一理賠要求。<br /><br />如需詳細資訊，請查看[使用傳送群組成員資格理賠要求規則以](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。|-發行群組宣告 – 不 account 查詢快速效能|使用者必須 Active Directory 本機群組成員|  
|傳送主張使用自訂規則|用來建立，以提供更多 [進階的選項] 比一般規則範本自訂規則。 您撰寫 AD FS 使用的自訂規則取得規則語言。<br /><br />如需詳細資訊，請查看[使用自訂理賠要求規則](When-to-Use-a-Custom-Claim-Rule.md)。|-可以用來來源 SQL 屬性網上商店的宣告<br />-可用來指定自訂 LDAP 篩選<br />-用以發出 PPID<br />-可以搭配自訂屬性網上商店<br />-可用來新增輸入的宣告設定，才能宣告<br />-可以用來傳送宣告根據多個連入宣告|-更難設定 \-可能需要一些時間道最初深入瞭解理賠要求規則語言|  
|允許或拒絕根據連入宣告使用者|用來建立規則允許或拒絕信賴，根據類型及值，連入理賠要求的使用者存取。<br /><br />如需詳細資訊，請查看[使用授權理賠要求規則](When-to-Use-an-Authorization-Claim-Rule.md)。|-簡化授權程序|-需要只有一個宣告類型，另一個取得值指定<br />-不支援宣告值對應模式|  
|允許所有使用者|用來建立，允許所有使用者存取信賴規則。<br /><br />如需詳細資訊，請查看[使用授權理賠要求規則](When-to-Use-an-Authorization-Claim-Rule.md)。|-簡單設定|-不太安全比使用允許] 或 [拒絕使用者型連入宣告範本|  
  

