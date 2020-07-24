---
title: AD 樹系復原-清除已移除 dc 的中繼資料
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: 4bf3ec5cb9495e3603c3a5a385f0ff7b65e9d8b7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962941"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>AD 樹系復原-清除已移除可寫入網域控制站的中繼資料

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

中繼資料清除會將識別 DC 的 Active Directory 資料移除至複寫系統。  

使用下列程式來刪除 dc 的 DC 物件，您打算透過重新安裝 AD DS，將其新增回網路。  
  
如果您使用的是 Active Directory 的使用者和電腦版本，或是包含遠端伺服器管理工具（RSAT） Active Directory 的網站和服務，則當您刪除 DC 物件時，會自動執行中繼資料清除。  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>使用 Active Directory 的使用者和電腦刪除網域控制站

當您使用 Active Directory 的使用者和電腦版本，或遠端伺服器管理工具（RSAT）中的 Active Directory 管理中心時，會在您刪除 DC 物件時自動執行中繼資料清除。 伺服器物件和 computer 物件也會自動刪除。  

或者，您也可以使用 RSAT 中 Active Directory 的網站和服務來刪除 DC 物件。 如果您使用 Active Directory 的網站和服務，則必須先刪除相關聯的伺服器物件和 NTDS 設定物件，才能刪除 DC 物件。  

如需安裝 RSAT 的詳細資訊，請參閱[遠端伺服器管理工具](../../../remote/remote-server-administration-tools.md)一文。
  
下列程式對執行 Windows Server 2016、2012、2008 R2 或2008的 Dc 而言是相同的。 中繼資料清除作業的目標 DC 可以執行任何版本的 Windows Server。  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>使用 RSAT 中的 Active Directory 使用者和電腦刪除網域控制站物件  
  
1. 依序按一下 [開始]****、[系統管理工具]**** 及 [Active Directory 使用者和電腦]****。  
2. 在主控台樹中，按兩下網域容器，然後按兩下 [**網域控制站**] 組織單位（OU）。  
3. 在詳細資料窗格中，在您要刪除的 DC 上按一下滑鼠右鍵，然後按一下 [**刪除**]。
   ![刪除](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. 按一下 [**是**] 確認刪除。 選取 [**此網域控制站已永久離線，而且無法再使用 Active Directory Domain Services 安裝精靈（DCPROMO）** ] 核取方塊來降級，然後按一下 [**刪除**]。  
5. 如果 DC 是通用類別目錄伺服器，請按一下 [**是]** 確認刪除。  

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
