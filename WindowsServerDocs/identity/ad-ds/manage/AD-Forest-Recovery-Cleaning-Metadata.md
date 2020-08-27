---
title: AD 樹系修復-清除已移除 dc 的中繼資料
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.openlocfilehash: ae95364ffa09a385e2fa03d630536165f50697b5
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938958"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>AD 樹系復原-清除已移除可寫入網域控制站的中繼資料

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

中繼資料清除會移除向複寫系統識別 DC 的 Active Directory 資料。

您可以使用下列程式，藉由重新安裝 AD DS，刪除您計畫要新增回網路的 dc 的 DC 物件。

如果您使用的 Active Directory 消費者和電腦版本或 Active Directory 包含遠端伺服器管理工具 (RSAT) 中的網站和服務，則在刪除 DC 物件時，會自動執行中繼資料清除。

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>使用 Active Directory 消費者和電腦刪除網域控制站

當您使用遠端伺服器管理工具 (RSAT) 中的 Active Directory 消費者和電腦或 Active Directory 管理中心版本時，當您刪除 DC 物件時，會自動執行中繼資料清除。 伺服器物件和電腦物件也會自動刪除。

或者，您也可以在 RSAT 中使用 Active Directory 網站和服務來刪除 DC 物件。 如果您使用 Active Directory 的網站和服務，您必須先刪除相關聯的伺服器物件和 NTDS 設定物件，才能刪除 DC 物件。

如需有關安裝 RSAT 的詳細資訊，請參閱 [遠端伺服器管理工具](../../../remote/remote-server-administration-tools.md)的文章。

針對執行 Windows Server 2016、2012、2008 R2 或2008的 Dc，下列程式是相同的。 中繼資料清除作業的目標 DC 可以執行任何版本的 Windows Server。

### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>在 RSAT 中使用 Active Directory 消費者和電腦刪除網域控制站物件

1. 依序按一下 [開始]****、[系統管理工具]**** 及 [Active Directory 使用者和電腦]****。
2. 在主控台樹中，按兩下 [網域] 容器，然後按兩下 [ **網域控制站** ] 組織單位 (OU) 。
3. 在詳細資料窗格中，以滑鼠右鍵按一下您要刪除的 DC，然後按一下 [ **刪除**]。
   ![刪除](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png)
4. 按一下 [**是**] 確認刪除。 選取 [ **此網域控制站已永久離線，而且無法再使用 Active Directory Domain Services 安裝精靈 (DCPROMO) ** ] 核取方塊來降級，然後按一下 [ **刪除**]。
5. 如果 DC 是通用類別目錄伺服器，請按一下 [ **是]** 確認刪除。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
