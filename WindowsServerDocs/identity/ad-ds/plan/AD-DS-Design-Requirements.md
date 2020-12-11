---
description: 深入瞭解： AD DS 設計需求
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: AD DS 設計需求
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 244f11e30e5c17f86dab6e8eee8ff8313e9d065f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050146"
---
# <a name="ad-ds-design-requirements"></a>AD DS 設計需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="designing-the-active-directory-logical-structure"></a>設計 Active Directory 邏輯結構
部署 Windows Server 2008 Active Directory Domain Services (AD DS) 之前，您必須規劃並設計環境的 AD DS 邏輯結構。 AD DS 邏輯結構會決定目錄物件的組織方式，並提供有效的方法來管理您的網路帳戶和共用資源。 當您設計 AD DS 邏輯結構時，您會定義組織網路基礎結構的重要部分。

若要設計 AD DS 邏輯結構，請判斷您的組織所需的樹係數目，然後建立網域、網域名稱系統 (DNS) 基礎結構和組織單位 (Ou) 的設計。 下圖顯示設計邏輯結構的程式。

![AD DS 設計需求](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)

如需詳細資訊，請參閱 [設計 Windows Server 2008 的邏輯結構 AD DS](Designing-the-Logical-Structure.md)。

## <a name="designing-the-site-topology"></a>設計網站拓撲
在您設計 AD DS 基礎結構的邏輯結構之後，您必須為您的網路設計網站拓撲。 網站拓撲是實體網路的邏輯標記法。 其中包含 AD DS 網站位置的相關資訊、每個網站中的 AD DS 網域控制站，以及支援網站間 AD DS 複寫的站台連結和站台連結橋接器。 下圖顯示網站拓撲設計流程。

![AD DS 設計需求](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)

如需詳細資訊，請參閱 [設計適用于 Windows Server 2008 的網站拓撲 AD DS](Designing-the-Site-Topology.md)。

## <a name="planning-domain-controller-capacity"></a>規劃網域控制站容量
為了確保有效率的 AD DS 效能，您必須為每個網站判斷適當的網域控制站數目，並確認它們符合 Windows Server 2008 的硬體需求。 謹慎規劃網域控制站的容量，可確保您不低估硬體需求，這可能會導致網域控制站效能不良和應用程式回應時間變慢。 下圖顯示網域控制站容量規劃的流程。

![AD DS 設計需求](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)

## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>啟用 Windows Server 2008 advanced AD DS 功能
您可以使用 Windows Server 2008 AD DS 藉由提高網域或樹系功能等級，將先進的功能引進您的環境。 當網域或樹系中的所有網域控制站都執行 Windows Server 2008 時，您可以將功能等級提高至 Windows Server 2008。

如需詳細資訊，請參閱 [啟用 AD DS 的 Advanced 功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)。



