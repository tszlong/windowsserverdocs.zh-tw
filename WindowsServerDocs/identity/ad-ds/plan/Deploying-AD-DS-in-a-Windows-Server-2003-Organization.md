---
description: 深入瞭解：在 Windows Server 2003 組織中部署 AD DS
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: 在 Windows Server 2003 組織中部署 AD DS
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4aef979fe6dc3fc59bcfb3d6e97da943ee6b2187
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047076"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>在 Windows Server 2003 組織中部署 AD DS

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織目前執行 Windows Server 2003 Active Directory，您可以將 Windows Server 2008 Active Directory Domain Services (AD DS) ，方法是將部分或全部網域控制站的作業系統就地升級到 Windows Server 2008，或是將執行 Windows Server 2008 的網域控制站引進您的環境。

在您可以將執行 Windows Server 2008 的網域控制站新增至現有的 Windows Server 2003 Active Directory 網域之前，您必須先執行 **adprep**（命令列工具）。 Adprep 會延伸 AD DS 架構、更新所選物件的預設安全描述項，以及新增一些應用程式所需的新目錄物件。 您可以在 Windows Server 2008 安裝磁片上取得 Adprep ( # A0) 。 如需詳細資訊，請參閱 [Adprep](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731728(v=ws.11))。

下圖顯示在目前正在執行 Windows Server 2003 Active Directory 的網路環境中部署 Windows Server 2008 AD DS 的步驟。

![在 windows 2003 組織中部署](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)

> [!NOTE]
> 如果您想要將網域或樹系功能等級設定為 Windows Server 2008，您環境中的所有網域控制站都必須執行 Windows Server 2008 作業系統。

將從 Windows Server 2003 環境就地升級的資源網域和帳戶網域合併為 Windows Server 2008 AD DS 部署，可能需要樹服務或林內網域重建。 在樹系之間重建 AD DS 網域可協助您降低組織在 AD DS 中的表示複雜度，並有助於降低相關的系統管理成本。 在樹系中重建 AD DS 網域可協助您減少複寫流量、減少所需的使用者和群組管理量，以及簡化群組原則的管理，以降低組織的系統管理負擔。 如需詳細資訊，請參閱 [ADMT 指南：遷移和重建 Active Directory 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10))。

如需您可以用來在執行 Windows Server 2003 Active Directory 的組織中規劃和部署 AD DS 的詳細工作清單，請參閱 [檢查清單：在 Windows server 2003 組織中部署 AD DS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771407(v=ws.10))。
