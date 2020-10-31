---
ms.assetid: 7e87ce2b-062e-479f-bcf2-585b6c42026a
title: 身分識別元件更新
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 8187be75c8f7ab537e6ae6e140d099c8f2a95d9b
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070670"
---
# <a name="identity-component-updates"></a>身分識別元件更新

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="lesson-1-identity-component-updates"></a>第1課：身分識別元件更新
本課程說明 Windows Server 2012 R2 中的身分識別元件更新。

### <a name="what-you-will-learn"></a>學習內容
完成本課程之後，您將能夠：

-   描述下列變更：

    -   [SPN 和 UPN 的唯一性](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)

    -   [Winlogon 自動重新開機 Sign-On &#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)

    -   [TPM 金鑰證明](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)

    -   [CA 備份與還原 Windows PowerShell Cmdlet](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)

    -   [命令列程序稽核](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)

    -   [認證保護和管理](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn408190(v=ws.11))

**作者** ： Justin Turner，與 Windows 群組的資深支援擴大工程師

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

