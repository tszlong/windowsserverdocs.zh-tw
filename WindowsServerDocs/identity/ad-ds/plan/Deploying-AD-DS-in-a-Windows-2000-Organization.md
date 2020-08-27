---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: 在 Windows Server 2000 組織中部署 AD DS
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 664224168efd347e6ca391112bf6b838d1f1c29a
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938828"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>在 Windows Server 2000 組織中部署 AD DS

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織目前正在執行 Windows 2000 Active Directory，您可以透過將部分或所有網域控制站的作業系統就地升級到 Windows Server 2008，或將執行 Windows Server 2008 的網域控制站引進您的環境，以部署 Windows Server 2008 Active Directory Domain Services (AD DS) 。

在您可以將執行 Windows Server 2008 的網域控制站新增至現有的 Windows 2000 Active Directory 網域之前，您必須先執行 **adprep**（命令列工具）。 Adprep 會延伸 AD DS 架構、更新所選物件的預設安全描述項，以及新增一些應用程式所需的新目錄物件。 您可以在 Windows Server 2008 安裝磁片上取得 Adprep ( # A0) 。 如需詳細資訊，請參閱 [Adprep](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731728(v=ws.11))。

> [!NOTE]
> 如果您想要將現有的 Windows 2000 AD DS 網域控制站就地升級到 Windows Server 2008，您必須先將伺服器升級為 Windows Server 2003，然後將其升級為 Windows Server 2008。

下圖顯示在目前正在執行 Windows 2000 Active Directory 的網路環境中部署 Windows Server 2008 AD DS 的步驟。

![在 windows 2000 組織中部署](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)

> [!NOTE]
> 如果您想要將網域或樹系功能等級設定為 Windows Server 2008，您環境中的所有網域控制站都必須執行 Windows Server 2008 作業系統。

將從 Windows 2000 環境就地升級的資源和帳戶網域合併為 Windows Server 2008 AD DS 部署的一部分，可能需要樹服務或林內網域重建。 在樹系之間重建 AD DS 網域可協助您降低組織的複雜度和相關聯的系統管理成本。 在樹系中重建 AD DS 網域可協助您減少複寫流量、減少所需的使用者和群組管理量，以及簡化群組原則的管理，以降低組織的系統管理負擔。 如需詳細資訊，請參閱 [ADMT 指南：遷移和重建 Active Directory 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10))。

如需您可以用來在目前執行 Windows 2000 Active Directory 的組織中規劃和部署 AD DS 的詳細工作清單，請參閱 [檢查清單：在 Windows 2000 組織中部署 AD DS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732737(v=ws.10))。
