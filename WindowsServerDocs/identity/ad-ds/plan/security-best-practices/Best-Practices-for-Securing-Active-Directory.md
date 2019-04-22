---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: 保護 Active Directory 的最佳做法
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 972def668634e794908a3ff2933d038ae38be5d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817079"
---
# <a name="best-practices-for-securing-active-directory"></a>保護 Active Directory 的最佳做法

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本文提供從業人員觀點，並包含一組實用技術，可協助 IT 管理人員保護企業 Active Directory 環境。 Active Directory 在 IT 基礎結構中扮演了關鍵角色，並確保全域互聯環境中不同網路資源的協調和安全性。 討論的方法主要根據 Microsoft 資訊安全與風險管理 (ISRM) 組織的經驗，也就是負責保護 Microsoft IT 和其他 Microsoft 商務部門的資產選取的數字的 Microsoft Global 500 客戶。  
  
-   [執行摘要](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [簡介](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [危及系統安全的途徑](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [吸引人的帳戶認證竊取](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [減少 Active Directory 攻擊面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [實作最低權限系統管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [實作安全的系統管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [保護不受攻擊的網域控制站](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [監視 Active Directory 遭到危害的徵兆](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [稽核原則建議](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [規劃危害](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [維護更安全的環境](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [附錄](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [附錄 b:Active Directory 中具有特殊權限的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附錄 c:Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [附錄 e:保護 Active Directory 中的 Enterprise Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [附錄 f:保護 Active Directory 中的 Domain Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [附錄 g:保護 Active Directory 中的系統管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [附錄 h:保護本機系統管理員帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [附錄 i:建立管理帳戶的受保護的帳戶和 Active Directory 中的群組](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [附錄 l:要監視的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [附錄 m︰文件連結與建議閱讀資料](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


