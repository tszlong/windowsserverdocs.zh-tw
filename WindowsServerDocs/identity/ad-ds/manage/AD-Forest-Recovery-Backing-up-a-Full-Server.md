---
title: "廣告樹系修復-備份完整伺服器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: b1af97c2eb23d65c2d106906bc0f5bb1f10b23ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>廣告樹系修復-備份完整伺服器  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

因為它可以還原到不同的硬體或其他作業系統的執行個體的樹系修復準備建議 server 的完整備份。  您可以使用 Windows Server 備份」來執行 server 的完整備份。 

## <a name="windows-server-backup"></a>Windows Server 備份
預設不會安裝 Windows Server 備份。 在 Windows Server 2016 和 Windows Server 2012 R2，請遵循下列步驟來安裝它。

>[!NOTE]
>請留意，步驟會有些許 Windows Server 2016 和 Windows Server 2012 R2。

步驟來安裝 Windows Server 2008 和 Windows Server 2008 R2，請查看[安裝 Windows Server 備份](https://technet.microsoft.com/library/cc771232.aspx)。  

### <a name="to-install-windows-server-backup"></a>若要安裝 Windows Server 備份
1. 開放**伺服器管理員**，按一下 [**新增角色與功能**。
2. 在**新增角色與功能精靈**按**下**。
3. 在**安裝類型**畫面中，保留預設的 [**以角色為基礎，或為基礎的功能的安裝**按**下一步**。
4. 在**伺服器選取**畫面中，按一下 [**下**。
5. 在**伺服器角色**畫面上按一下**下**。
6. 在**功能**畫面上，選取**Windows Server 備份**並按**下一步**<ph x="4">
！[</ph>安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. 按一下**安裝**。
8. 安裝完成後，按一下**關閉**。


### <a name="to-perform-a-backup-with-windows-server-backup"></a>備份與 Windows Server 備份

1. 開放**伺服器管理員**，按一下 [**工具**，然後按一下 [ **Windows Server 備份**。
    - 在 Windows Server 2008 R2 和 Windows Server 2008，按一下**[開始]**，指向 [**系統管理工具**，然後按一下 [ **Windows Server 備份**。 
![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. 如果您的提示，請在**使用者 Account 控制項**對話方塊中，提供者備份認證，然後再按一下**[確定]**。
3. 按一下**本機備份**。
4. 在**動作**功能表上，按**備份一次**。
5. 備份一次精靈中，在**備份選項**頁面上，按一下 [**的其他不同選項**，然後按一下 [**下一步**。
![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. 在**選取備份設定**頁面上，按一下 [**完整伺服器（建議選項）**，然後按一下 [**下一步**。
7. 在**指定目的地類型**頁面上，按一下 [**本機磁碟機**或**遠端共用資料夾**，然後按一下 [**下一步**。
8. 在**選擇備份目的地**頁面上，選擇備份的位置。  如果您選取 [本機磁碟機選擇到本機磁碟機，或如果您選取 [遠端分享選擇網路共用。
9. 在確認畫面上，按一下 [**備份**。
![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)
10. 完成此按一下**關閉**。
11. 關閉 Windows Server 備份。

>[!NOTE]
>如果您收到錯誤，表示不的備份儲存位置可使用時，您將需要排除已選取的磁碟區的其中一個或新增新的磁碟區或遠端分享。
>如果您收到警告這部選取磁碟區也包含了要備份的項目清單中，判斷是否要移除，然後按一下**[確定]**。

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>備份 windows server 使用 Wbadmin.exe
Wbadmin.exe 是一個命令列的公用程式，可讓您備份及還原您的作業系統、磁碟區、檔案、資料夾和應用程式的命令提示字元。

#### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>若要執行使用 Wbadmin.exe server 的完整備份  
  
1.  打開提升權限的命令提示字元中，輸入下列命令並按下 ENTER:  

        wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:

![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
