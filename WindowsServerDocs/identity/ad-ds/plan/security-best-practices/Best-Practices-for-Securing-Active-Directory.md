---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: 保護 Active Directory 的最佳做法
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 444ba28da788ca5986b49eaf84675c1eacfe5522
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941538"
---
# <a name="best-practices-for-securing-active-directory"></a>保護 Active Directory 的最佳做法

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本檔提供實踐者的觀點，並包含一組可協助 IT 主管保護企業 Active Directory 環境的實用技巧。 Active Directory 在 IT 基礎結構中扮演了關鍵角色，並確保全域互聯環境中不同網路資源的協調和安全性。 討論的方法主要是以 Microsoft 資訊安全性和風險管理為基礎， (ISRM) 組織的經驗，除了建議所選的 Microsoft 全球500客戶之外，還負責保護 Microsoft IT 和其他 Microsoft 營業單位的資產。

-   [執行摘要](../../../ad-ds/manage/component-updates/Executive-Summary.md)

-   [簡介](../../../ad-ds/manage/component-updates/Introduction.md)

-   [危及系統安全的途徑](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)

-   [常成為認證竊取目標的帳戶](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)

-   [減少 Active Directory 攻擊面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)

-   [執行最低許可權的系統管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)

-   [實作安全的管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)

-   [保護網域控制站免于遭受攻擊](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)

-   [監視入侵徵兆 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)

-   [稽核原則建議](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)

-   [規劃入侵](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)

-   [維護更安全的環境](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)

-   [附錄](../../../ad-ds/plan/security-best-practices/Appendices.md)

-   [附錄 B：Active Directory 中具特殊權限的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)

-   [附錄 C：Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)

-   [附錄 D：保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)

-   [附錄 E：保護 Active Directory 中的 Enterprise Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)

-   [附錄 F：保護 Active Directory 中的 Domain Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)

-   [附錄 G：保護 Active Directory 中的 Administrators 群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)

-   [附錄 H：保護本機 Administrators 帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)

-   [附錄 I：為 Active Directory 中的受保護帳戶和群組建立管理帳戶](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)

-   [附錄 L：要監視的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)

-   [附錄 M：文件連結與建議閱讀資料](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)



