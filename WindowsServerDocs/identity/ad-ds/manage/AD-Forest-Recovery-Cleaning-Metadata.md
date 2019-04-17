---
title: "廣告樹系修復-清除已移除網域控制站中繼資料"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adfs
ms.openlocfilehash: 3027c59b58801b44d20127e6bcf62dd7319708bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>廣告樹系修復-清潔移除寫入網域控制站中繼資料 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
 
 中繼資料清除移除辨識 DC 系統複寫 Active Directory 資料。  
  
 使用下列程序的網域控制站想要重新安裝 AD DS 新增網路回到 delete DC 物件。  
  
 如果您正在使用 Active Directory 使用者和電腦的版本，或 Active Directory 網站和服務，包括遠端伺服器管理工具 (RSAT)，清除中繼資料會在您 delete 俠物件時自動執行。  
  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>刪除網域控制站使用 Active Directory 使用者與電腦  
 當您使用的 Active Directory 使用者或電腦 Active Directory 管理中心遠端伺服器管理工具 (RSAT) 版本時，清除中繼資料會在您 delete 俠物件時自動執行。 也會自動刪除伺服器物件與電腦物件。  
  
 或者，您也可以使用 Active Directory 網站和服務中 RSAT 到 delete DC 物件。 如果您使用 Active Directory 網站和服務，您必須之前，您可以 delete 俠物件 delete 相關聯的伺服器物件和 NTDS 設定物件。  
  
 若要下載 RSAT:  

-   [適用於 Windows 10 遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)
  
-   [遠端伺服器管理工具適用於 Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)  

-   [適用於 Windows 7 Service Pack 1 (SP1) 與遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=7887)  
  
-   [Microsoft 遠端伺服器管理工具適用於 Windows Vista](https://www.microsoft.com/download/details.aspx?id=21090)  
  
 下列程序也適用於執行的是 Windows Server 2016 2012 年、2008 R2 或 2008 年網域控制站。 中繼資料清除作業的目標俠可以執行任何的 Windows Server 版本。  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>若要 delete 網域控制站物件 RSAT 使用 Active Directory 使用者與電腦  
  
1.  按一下**[開始]**，按一下**系統管理工具]**，然後按一下 [ **Active Directory 使用者與電腦**。  
2.  在主控台按兩下網域控制站，，然後按兩下 [**網域控制站**單位（組織單位）。  
3.  在詳細資料窗格中，以滑鼠右鍵按一下您想要 delete，然後按一下 [DC **Delete**。 
![Delete](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4.  按一下**[是]**確認刪除。 選取 [**這個網域控制站永久離線，可以不會降級使用 Active Directory Domain Services 安裝精靈（帶領）**核取方塊，然後按一下 [ **Delete**。  
5.  如果 DC 是一個通用伺服器，請按一下**[是]**確認刪除。  
  
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
  
