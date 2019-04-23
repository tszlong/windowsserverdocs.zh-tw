---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: AD DS 設計需求
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 79c694112f39adf5d37cd28f6bd7a770dedf3976
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857519"
---
# <a name="ad-ds-design-requirements"></a>AD DS 設計需求

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>設計 Active Directory 邏輯結構  
部署 Windows Server 2008 Active Directory 網域服務 (AD DS) 之前，您必須規劃，並為您的環境中設計的 AD DS 邏輯結構。 AD DS 邏輯結構可讓您決定如何組織您的目錄物件，並提供用於管理您的網路帳戶和共用的資源的有效方法。 當您設計您的 AD DS 邏輯結構時，您會定義您的組織的網路基礎結構的重要部分。  
  
若要設計的 AD DS 邏輯結構，判斷您的組織要求的樹系的數目，並再建立的網域、 網域名稱系統 (DNS) 基礎結構，以及組織單位 (Ou) 的設計。 下圖顯示設計的邏輯結構的程序。  
  
![AD DS 飶耵獌炴](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
如需詳細資訊，請參閱 <<c0> [ 設計邏輯結構的 Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)。  
  
## <a name="designing-the-site-topology"></a>設計站台拓撲  
為您的 AD DS 基礎結構設計的邏輯結構之後，您必須設計為您的網路的站台拓撲。 站台拓樸是您的實體網路的邏輯表示法。 它包含位置的 AD DS 站台，每個站台，在站台連結和支援 AD DS 複寫站台之間的站台連結橋接器的 AD DS 網域控制站相關的資訊。 下圖顯示站台拓撲設計程序。  
  
![AD DS 飶耵獌炴](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
如需詳細資訊，請參閱 <<c0> [ 設計站台拓撲的 Windows Server 2008 AD DS](Designing-the-Site-Topology.md)。  
  
## <a name="planning-domain-controller-capacity"></a>規劃網域控制站容量  
若要確保有效率的 AD DS 效能，您必須判斷適當數目的每個站台的網域控制站，並確認它們符合 Windows Server 2008 的硬體需求。 請小心規劃您的網域控制站的容量可確保不會低估硬體需求，這可能會導致不佳的網域控制站效能和應用程式回應時間。 下圖顯示網域控制站容量規劃程的序。  
  
![AD DS 飶耵獌炴](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>啟用 Windows Server 2008 的進階 AD DS 功能  
您可以使用 Windows Server 2008 AD DS 引入您的環境中的進階的功能，藉由提升網域或樹系功能等級。 當網域或樹系中的所有網域控制站都執行 Windows Server 2008 時，您可以提高功能等級為 Windows Server 2008。  
  
如需詳細資訊，請參閱 <<c0> [ 啟用進階功能適用於 AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)。  
  


