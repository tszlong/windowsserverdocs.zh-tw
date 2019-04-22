---
title: AD 樹系復原-備份完整伺服器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: a5306960bb2dca3849bdb4fc7304781af3f25335
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815869"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>AD 樹系復原-備份系統狀態資料  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用下列程序來使用 Windows Server Backup 或 wbadmin.exe 在 DC 上執行系統狀態備份。  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>若要執行系統狀態備份使用 Windows Server Backup

1. 開啟**伺服器管理員**，按一下**工具**，然後按一下**Windows Server Backup**。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，按一下**開始**，指向**系統管理工具**，然後按一下**Windows Server Backup**。 

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. 如果系統提示您，在**使用者帳戶控制** 對話方塊中，提供備份操作員的認證，然後再按一下**確定**。
3. 按一下 **本機備份**。
4. 在 [執行] 功能表上按一下 [一次性備份]。
5. 在 單次備份精靈，在**備份選項**頁面上，按一下**不同的選項**，然後按一下**下一步**。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. 在**選取的備份組態**頁面上，按一下**自訂)**，然後按一下**下一步**。
7. 上**選取要備份項目**畫面上，按一下**新增的項目**，然後選取**系統狀態**，按一下  **Ok**。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，選取要包含在備份的磁碟區。 如果您選取**啟用系統修復**核取方塊，會選取所有重要磁碟區。 

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. 上**指定的目的型別**頁面上，按一下**本機磁碟機**或**遠端共用資料夾**，然後按一下**下一步**。  如果您要備份到遠端共用資料夾，執行下列作業：  
   - 輸入共用資料夾的路徑。
   - 底下**存取控制**，選取**不會繼承**或是**繼承**以決定是否可以存取備份，然後按一下 [**下一步]**。  
   - 在 **提供使用者認證來備份** 對話方塊中，共用資料夾中，擁有寫入權限的使用者提供的使用者名稱和密碼，然後按一下 **確定**。

9. Windows Server 2008 R2 和 Windows Server 2008 上**進階選項的指定**頁面上，選取**VSS 複製備份**，然後按一下 [**下一步]**。
10. 在 **選取備份目的地**頁面上，選擇備份位置。  如果您選取本機磁碟機選擇本機的磁碟機，或如果您選取 遠端共用選擇網路共用。
11. 在 確認 畫面中，按一下 **備份**。
12. 一旦完成此作業後按一下 **關閉**。
13. 關閉 Windows Server Backup。

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>若要執行使用 Wbadmin.exe 的系統狀態備份

開啟提升權限的命令提示字元中，輸入下列命令，然後按 ENTER:  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
