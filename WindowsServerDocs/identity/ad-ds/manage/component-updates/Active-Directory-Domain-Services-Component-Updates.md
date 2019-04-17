---
title: "Active Directory Domain 服務的元件更新"
description: "本文件討論 AD DS 元件更新針對 Windows Server 2012 R2"
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 52d3dab542b4250670067e927f42ddf1597fc852
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-domain-services-component-updates"></a>Active Directory Domain 服務的元件更新

>適用於：Windows Server 2016、Windows Server 2012 R2

這個模組導入收到次要更新身分空間與 Directory 服務的元件。  
  
|關於作者|  
|--------------------|  
|**作者：**|Justin Turner|  
|**自傳：**|Justin 是資深支援工程師 Directory 服務小組以 Irving，德州，USA。  他建立或過去 12 年來提供許多訓練課程及知識庫文章適用於 Microsoft 知識庫。 他教導 Microsoft 員工，針對新 product 架構許可 Microsoft 認證主機 (MCM) Microsoft 認證訓練 (MCT)，會 M.S. 在電腦教育版和系統辨識度。|  
|**參與者**|這個訓練模組包含貢獻從*Michiko 簡短*，*...改良*，*心靈 Jowett*，*Manu Pushpendran*，*Yashar Bahman*，*Anoosh Saboori*、*Rashmi Jha*，*Justin 恐怖*和*Herbert Mauerer*|  
|**檢閱**|非常感謝自己時間審查及提供意見反應所花費的下列個人：*Joey Seifert*，*Justin 恐怖*|  
  
> [!NOTE]  
> 本文由 Microsoft 客戶支援工程師撰寫，以及適用於系統管理員經驗和系統設計師超過參考 TechNet 上的主題通常會提供深入的技術解釋的功能與 Windows Server 2012 R2 方案正在尋找。 不過，尚未經歷相同編輯行程，以便某些語言的似乎比哪些通常位於 TechNet 較少的外觀。  
  
### <a name="what-you-will-learn"></a>您會了解  
完成此模組之後，您將可以：  
  
-   解釋對 Windows Server 2012 R2 的 Directory 服務和身分技術區域中的元件更新  
  
    -   [SPN 和 UPN 唯一性](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Winlogon 自動重新登入和 #40;ARSO 與 #41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [TPM 金鑰證明](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [CA 備份及還原 Windows PowerShell cmdlet](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [命令列處理程序稽核](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [認證保護與管理](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [Directory 服務的元件更新](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [網域和森林功能層級](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [取代 NTFRS 了](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [變更 LDAP 最佳化](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [1644 事件改良功能](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Active Directory 複寫輸送量改進](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


