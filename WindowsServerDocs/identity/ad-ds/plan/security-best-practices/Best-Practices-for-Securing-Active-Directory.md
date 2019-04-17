---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: "保護 Active Directory 最佳做法"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ecef0c173677d379524189b1769d4721ab0774a8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="best-practices-for-securing-active-directory"></a>保護 Active Directory 最佳做法

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本文件會提供的從業員透視圖及包含的實際技術，以協助保護 Active Directory 企業環境 IT 主管一組。 Active Directory 中的 IT 基礎結構，播放重要的角色，並確保協調了和安全性的全域、 互相連接的環境中不同的網路資源。 主要是 Microsoft 資訊安全和風險管理 (ISRM) 組織的體驗，這是負責保護的 Microsoft IT 和其他 Microsoft 商務部門，除了通知數目 Microsoft 全球 500 針對基礎討論的方法。  
  
-   [前言](https://technet.microsoft.com/library/dn487451.aspx)  
  
-   [通知](https://technet.microsoft.com/library/dn487445.aspx)  
  
-   [執行摘要](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [簡介](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [途徑危害](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [認證竊取帳號吸引](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [減少 Active Directory 攻擊](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [實作最低權限管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [實作安全管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [保護網域控制站攻擊](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Active Directory 監視危害的符號](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [稽核原則建議](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [規劃區域的入侵](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [維護更安全的環境](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [附錄](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [B 附錄特殊權限的帳號及 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [C：附錄受保護的帳號及 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [在 Active Directory 中附錄 d 保護建系統管理員帳號](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [在 Active Directory 中附錄 e 保護企業管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [在 Active Directory 中附錄 f︰ 保護網域管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [在 Active Directory 中附錄 g：保護系統管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [附錄 H:WINDOWS 保護本機系統管理員帳號，並群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [附錄 i：建立管理帳號受保護的帳號和 Active Directory 中的群組](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [事件監視器附錄 l:](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [附錄 m：文件的連結，並建議朗讀](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


