---
title: Active Directory 網域服務元件更新
description: 本文件討論 Windows Server 2012 r2 的 AD DS 元件更新
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 300bf7dafe0ef0ede754302f2c0aa8c36a7adbfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889879"
---
# <a name="active-directory-domain-services-component-updates"></a>Active Directory 網域服務元件更新

>適用於：Windows Server 2016, Windows Server 2012 R2

本單元介紹目錄服務和身分識別領域中收到小幅更新的元件。  
  
|關於作者|  
|--------------------|  
|**作者：**|Justin Turner|  
|**個人簡歷：**|Justin 是目錄服務小組的資深支援升級工程師，工作地點在美國德州的 Irving 市。  他在過去 12 年已建立或參與 Microsoft 知識庫的眾多培訓課程和知識庫文章。 他教授 Microsoft 員工和客戶新的產品架構，是 Microsoft Certified Master (MCM)、 Microsoft 認證講師 (MCT) 規範，並保留 M.S. 在 教育和認知系統的程度。|  
|**參與者**|本訓練單元的參與者包括 *Michiko Short*、*Dean Wells*、*Alan Jowett*、*Manu Pushpendran*、*Yashar Bahman*、*Anoosh Saboori*、*Rashmi Jha*、*Justin Hall* 和 *Herbert Mauerer*|  
|**審閱者**|非常感謝下列自己花費時間檢閱並提供意見反應：*Joey Seifert*， *Justin Hall*|  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
### <a name="what-you-will-learn"></a>您將學到  
完成本單元後，您將能夠：  
  
-   說明 Windows Server 2012 R2 中的目錄服務和身分識別技術領域所進行的元件更新。  
  
    -   [SPN 和 UPN 的唯一性](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Winlogon 自動重新啟動登入&#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [TPM 金鑰證明](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [CA 備份與還原 Windows PowerShell Cmdlet](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [命令列程序稽核](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [認證保護和管理](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [目錄服務元件更新](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [網域與樹系功能等級](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [NTFRS 過時](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [LDAP 查詢最佳化工具變更](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [1644 事件改進](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Active Directory 複寫輸送量改進](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


