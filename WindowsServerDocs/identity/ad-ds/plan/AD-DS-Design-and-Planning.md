---
description: 深入瞭解： AD DS 的設計和規劃
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: AD DS 設計與規劃
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 92ab8a4e3225c62fd526902c312aa889ee6831d1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050136"
---
# <a name="ad-ds-design-and-planning"></a>AD DS 設計與規劃

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由在您的環境中部署 Windows Server Active Directory Domain Services (AD DS) ，您可以利用 (提供的集中式、委派的系統管理模型和單一登入) SSO AD DS 功能。 找出組織的部署工作和目前的環境之後，您可以建立符合組織需求的 AD DS 部署策略。

## <a name="about-this-guide"></a>關於本指南

本指南提供的建議可協助您根據組織的需求和您想要建立的特定設計來開發 AD DS 部署策略。 本指南適用於基礎結構專家或系統架構設計人員使用。 閱讀本指南之前，您應該先充分瞭解 AD DS 在功能等級上的運作方式。 您也應該對將反映在 AD DS 部署策略中的組織需求有充分的瞭解。

本指南說明 Windows Server 2008 AD DS 部署的數個可能起點的工作集。 本指南可協助您決定最適合您環境的部署策略。

雖然本指南中所提供的策略適用于幾乎所有的伺服器作業系統部署，但這些策略已特別針對包含少於100000位使用者和少於1000個網站的環境進行測試和驗證，且網路連接最少每秒 28.8 kb (Kbps) 。 如果您的環境不符合這些準則，請考慮使用在更複雜的環境中部署 AD DS 經驗的諮詢公司。

如需有關測試 AD DS 部署程式的詳細資訊，請參閱 [測試和驗證部署](/previous-versions/windows/it-pro/windows-server-2003/cc772722(v=ws.10))程式一文。

## <a name="in-this-guide"></a>本指南內容

[了解 AD DS 設計](Understanding-AD-DS-Design.md)

[識別您的 AD DS 設計與部署需求](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)

[將您的需求和 AD DS 部署策略對應](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)

[設計 Windows Server 2008 AD DS 的邏輯結構](Designing-the-Logical-Structure.md)

[設計 Windows Server 2008 AD DS 的站台拓撲](Designing-the-Site-Topology.md)

[啟用 AD DS 的進階功能](Enabling-Advanced-Features-for-AD-DS.md)

[評估 AD DS 部署策略範例](Evaluating-AD-DS-Deployment-Strategy-Examples.md)

[附錄 A：檢視重要的 AD DS 術語](Appendix-A--Reviewing-Key-AD-DS-Terms.md)
