---
title: Active Directory 網域服務元件更新
description: 本檔討論 Windows Server 2012 R2 的 AD DS 元件更新
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 09/08/2017
ms.topic: article
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.openlocfilehash: a5ed47dae3cab9c026656fcccedae30a42325681
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069800"
---
# <a name="active-directory-domain-services-component-updates"></a>Active Directory 網域服務元件更新

>適用于： Windows Server 2016、Windows Server 2012 R2

本單元介紹目錄服務和身分識別領域中收到小幅更新的元件。


| 關於作者 |
|------------------|
|   **作者：**    |
|     **個人簡歷：**     |
| **參與者** |
|  **審閱者**   |

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

### <a name="what-you-will-learn"></a>學習內容
完成本單元後，您將能夠：

-   說明 Windows Server 2012 R2 中的目錄服務和身分識別技術領域所進行的元件更新。

    -   [SPN 和 UPN 的唯一性](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)

    -   [Winlogon 自動重新開機 Sign-On &#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)

    -   [TPM 金鑰證明](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)

    -   [CA 備份與還原 Windows PowerShell Cmdlet](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)

    -   [命令列程序稽核](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)

    -   [認證保護和管理](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn408190(v=ws.11))

    -   [目錄服務元件更新](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)

        -   [網域與樹系功能等級](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)

        -   [NTFRS 過時](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)

        -   [LDAP 查詢最佳化工具變更](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)

        -   [1644 事件改進](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)

        -   [Active Directory 複寫輸送量改進](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)
