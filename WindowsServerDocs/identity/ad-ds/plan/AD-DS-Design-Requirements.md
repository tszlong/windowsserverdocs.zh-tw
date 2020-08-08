---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: AD DS 設計需求
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d81a6bfb3ae29b4a9c76c5e902e8fec8e08c84b0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941541"
---
# <a name="ad-ds-design-requirements"></a>AD DS 設計需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="designing-the-active-directory-logical-structure"></a>設計 Active Directory 邏輯結構
在部署 Windows Server 2008 Active Directory Domain Services (AD DS) 之前，您必須為您的環境規劃及設計 AD DS 的邏輯結構。 AD DS 邏輯結構會決定目錄物件的組織方式，並提供有效的方法來管理您的網路帳戶和共用資源。 當您設計 AD DS 邏輯結構時，您會定義組織網路基礎結構的重要部分。

若要設計 AD DS 邏輯結構，請決定您的組織所需的樹係數目，然後建立網域的設計、網域名稱系統 (DNS) 基礎結構，以及 (Ou) 的組織單位。 下圖顯示設計邏輯結構的程式。

![AD DS 設計需求](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)

如需詳細資訊，請參閱[設計 Windows Server 2008 的邏輯結構 AD DS](Designing-the-Logical-Structure.md)。

## <a name="designing-the-site-topology"></a>設計網站拓撲
在設計 AD DS 基礎結構的邏輯結構之後，您必須為您的網路設計網站拓撲。 網站拓撲是實體網路的邏輯標記法。 其中包含 AD DS 網站位置、每個網站內 AD DS 網域控制站，以及支援網站間 AD DS 複寫的站台連結和站台連結橋的相關資訊。 下圖顯示網站拓朴設計流程。

![AD DS 設計需求](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)

如需詳細資訊，請參閱[設計 Windows Server 2008 的網站拓朴 AD DS](Designing-the-Site-Topology.md)。

## <a name="planning-domain-controller-capacity"></a>規劃網域控制站容量
為確保有效率的 AD DS 效能，您必須為每個網站判斷適當的網域控制站數目，並確認其符合 Windows Server 2008 的硬體需求。 請小心，您的網域控制站的容量規劃可確保您不會低估硬體需求，這可能會導致網域控制站效能和應用程式回應時間不佳。 下圖顯示網域控制站容量規劃的程式。

![AD DS 設計需求](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)

## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>啟用 Windows Server 2008 advanced AD DS 功能
您可以藉由提高網域或樹系功能等級，使用 Windows Server 2008 AD DS 將先進的功能引進您的環境中。 當網域或樹系中的所有網域控制站都執行 Windows Server 2008 時，您可以將功能等級提升至 Windows Server 2008。

如需詳細資訊，請參閱[啟用 AD DS 的先進功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)。



