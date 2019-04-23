---
title: AD 樹系復原-備份完整伺服器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: fec8de8ea1dadb392f6a3bd1c881e8df2266f404
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846519"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>AD 樹系復原-備份完整伺服器  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

完整伺服器備份，建議準備樹系復原，因為它可以還原至不同的硬體或不同的作業系統執行個體。  您可以使用 Windows Server Backup 執行完整備份伺服器。 

## <a name="windows-server-backup"></a>Windows Server Backup

預設不會安裝 Windows Server Backup。 在 Windows Server 2016 和 Windows Server 2012 R2，請遵循下列步驟來安裝。

>[!NOTE]
>請留意步驟可能略有不同，Windows Server 2016 和 Windows Server 2012 R2 之間。

如需安裝 Windows Server 2008 和 Windows Server 2008 R2 中的步驟，請參閱[安裝 Windows Server Backup](https://technet.microsoft.com/library/cc771232.aspx)。  

### <a name="to-install-windows-server-backup"></a>若要安裝 Windows Server Backup

1. 開啟**伺服器管理員**然後按一下**新增角色及功能**。
2. 在 **新增角色及功能精靈**按一下 **下一步**。
3. 在 **安裝類型**畫面上，保留預設值**角色型或功能型安裝**，按一下 **下一步**。
4. 在 [**伺服器選取項目**畫面上，按一下**下一步]**。
5. 在 [**伺服器角色**畫面上，按一下**下一步]**。
6. 在 [**功能**畫面上，選取**Windows Server Backup**然後按一下**下一步]**
   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. 按一下 **\[安裝\]**。
8. 安裝完成後，按一下**關閉**。

### <a name="to-perform-a-backup-with-windows-server-backup"></a>使用 Windows Server Backup 將資料備份

1. 開啟**伺服器管理員**，按一下**工具**，然後按一下**Windows Server Backup**。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，按一下**開始**，指向**系統管理工具**，然後按一下**Windows Server Backup**。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. 如果系統提示您，在**使用者帳戶控制** 對話方塊中，提供備份操作員的認證，然後再按一下**確定**。
3. 按一下 **本機備份**。
4. 在 [執行] 功能表上按一下 [一次性備份]。
5. 在 單次備份精靈，在**備份選項**頁面上，按一下**不同的選項**，然後按一下**下一步**。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. 上**選取的備份組態**頁面上，按一下 **（建議選項） 的完整伺服器**，然後按一下**下一步**。
7. 上**指定的目的型別**頁面上，按一下**本機磁碟機**或**遠端共用資料夾**，然後按一下**下一步**。
8. 在 **選取備份目的地**頁面上，選擇備份位置。  如果您選取本機磁碟機選擇本機的磁碟機，或如果您選取 遠端共用選擇網路共用。
9. 在 確認 畫面中，按一下 **備份**。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. 一旦完成此作業後按一下 **關閉**。
11. 關閉 Windows Server Backup。

>[!NOTE]
>如果您收到錯誤，指出沒有備份的儲存位置不在可用時，您必須排除其中一個已選取的磁碟區，或新增新的磁碟區或遠端共用。
>如果您收到警告，指出選取的磁碟區也包含在要備份項目清單中，判斷是否要移除，然後按一下**Ok**。

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>使用 Wbadmin.exe 來備份 windows server

Wbadmin.exe 是命令列公用程式，可讓您備份及還原您的作業系統、 磁碟區、 檔案、 資料夾和應用程式，從命令提示字元。

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>若要執行使用 Wbadmin.exe 的完整伺服器備份
  
- 開啟提升權限的命令提示字元中，輸入下列命令，然後按 ENTER:  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
