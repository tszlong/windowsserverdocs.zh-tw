---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: "AD DS 設計需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 17338cd00fecec098865095dd9613f62beb3a457
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="ad-ds-design-requirements"></a>AD DS 設計需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>設計的 Active Directory 邏輯結構  
部署 Windows Server 2008 Active Directory Domain Services (AD DS) 之前，您必須計劃，並為您的環境設計 AD DS 邏輯結構。 AD DS 邏輯結構判斷組織 directory 物件的方式，並提供有效的方法管理帳號網路和共用的資源。 當您設計您 AD DS 邏輯結構時，您定義重大網路基礎結構貴組織的一部分。  
  
若要設計 AD DS 邏輯結構，判斷您的組織所需的樹系的數字，然後建立網域、基礎結構網域名稱系統」(DNS) 和組織單位 (Ou) 的設計。 下圖顯示設計的邏輯結構程的序。  
  
![AD DS 設計需求](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
如需詳細資訊，請查看[設計邏輯結構適用於 Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)。  
  
## <a name="designing-the-site-topology"></a>設計網站拓撲  
您的基礎結構 AD DS 設計邏輯結構後，您必須設計網站拓撲為您的網路。 網站拓撲是邏輯表示您的實體網路。 它包含 AD DS 網域控制站在每個網站的網站的連結和支援 AD DS 複寫之間網站的網站連結橋接器 AD DS 網站的位置的相關資訊。 下圖顯示網站拓撲設計程序。  
  
![AD DS 設計需求](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
如需詳細資訊，請查看[設計網站拓撲適用於 Windows Server 2008 AD DS](Designing-the-Site-Topology.md)。  
  
## <a name="planning-domain-controller-capacity"></a>規劃網域控制站容量  
為了確保有效率 AD DS，您必須判斷適當數目的網域控制站的每個網站，並確認它們符合 Windows Server 2008 的硬體需求。 規劃網域控制站謹慎容量可確保您請勿低估硬體需求，可能會造成不佳的網域控制站效能和應用程式的回應時間。 下圖顯示程序的網域控制站容量計劃。  
  
![AD DS 設計需求](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>讓 Windows Server 2008 進階功能 AD DS  
您可以使用 Windows Server 2008 AD DS，您的環境進階的功能引入網域或森林功能層級。 執行 Windows Server 2008 網域或森林中的所有網域控制站時，您可以提高以 Windows Server 2008 的功能層級。  
  
如需詳細資訊，請查看[讓進階功能 AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)。  
  


