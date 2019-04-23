---
title: AD 樹系復原-清除已移除網域控制站的中繼資料
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: b71cab51a362a96ab6071e5eed3cf31c4421041c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843039"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>AD 樹系復原-清除已移除的可寫入網域控制站的中繼資料

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

中繼資料清除作業中移除識別複寫系統 DC 的 Active Directory 資料。  

您可以使用下列程序，刪除您計劃透過重新安裝 AD DS 新增至網路的網域控制站的 DC 物件。  
  
如果您使用 Active Directory 使用者和電腦的版本，或 Active Directory 站台與服務所包含的遠端伺服器管理工具 (RSAT)，中繼資料清除作業會在您刪除 DC 物件時自動執行。  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>刪除使用 Active Directory 使用者和電腦的網域控制站

當您使用 Active Directory 使用者和電腦或 Active Directory 管理中心遠端伺服器管理工具 (RSAT) 的版本時，中繼資料清除作業會在您刪除 DC 物件時自動執行。 伺服器物件的電腦物件也會自動刪除。  

或者，您也可以使用 Active Directory 站台和服務在 RSAT 中刪除的 DC 物件。 如果您使用 Active Directory 站台及服務，您必須刪除相關聯的伺服器物件和 NTDS 設定物件，才能刪除的 DC 物件。  

如需安裝 RSAT 的詳細資訊，請參閱文章[遠端伺服器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。
  
下列程序是相同的執行任一 Windows Server 2016、 2012年、 2008 R2 或 2008 Dc。 中繼資料清除作業的目標 DC 可以執行任何版本的 Windows Server。  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>若要刪除使用 RSAT 中的 Active Directory 使用者和電腦的網域控制站物件  
  
1. 依序按一下 [開始]、[系統管理工具] 及 [Active Directory 使用者和電腦]。  
2. 在主控台樹狀目錄中，依序按兩下網域容器，及**網域控制站**組織單位 (OU)。  
3. 在 詳細資料 窗格中，以滑鼠右鍵按一下您想要刪除此項目，然後按一下 DC**刪除**。
   ![刪除](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. 按一下 [**是**] 確認刪除。 選取 **此網域控制站已永久離線，而且可以不再被降級使用 Active Directory 網域服務安裝精靈 (DCPROMO)** 核取方塊，然後按一下**刪除**。  
5. 如果 DC 是通用類別目錄伺服器，請按一下**是**確認刪除。  

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
