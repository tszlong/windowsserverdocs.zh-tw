---
title: AD 樹系復原-備份完整伺服器
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.openlocfilehash: 93771bcac84680f07a39ce2791b67c2b35aff8dc
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067960"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>AD 樹系復原-備份完整伺服器

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

建議進行完整的伺服器備份，以準備進行樹系復原，因為它可以還原至不同的硬體或不同的作業系統實例。  您可以使用 Windows Server Backup 執行伺服器的完整備份。

## <a name="windows-server-backup"></a>Windows Server Backup

預設不會安裝 Windows Server Backup。 在 Windows Server 2016 和 Windows Server 2012 R2 中，遵循下列步驟來安裝它。

>[!NOTE]
>請注意，Windows Server 2016 和 Windows Server 2012 R2 之間的步驟可能稍有不同。

如需在 Windows Server 2008 和 Windows Server 2008 R2 中安裝它的步驟，請參閱 [安裝 Windows Server Backup](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771232(v=ws.10))。

### <a name="to-install-windows-server-backup"></a>若要安裝 Windows Server Backup

1. 開啟 **伺服器管理員** ，然後按一下 [ **新增角色及功能** ]。
2. 在 [ **新增角色及功能]** 上，按 **[下一步** ]。
3. 在 [ **安裝類型** ] 畫面上，保留預設 **角色型或以功能為基礎的安裝** ，然後按 **[下一步]** 。
4. 在 [ **伺服器選取** ] 畫面上，按 **[下一步]** 。
5. 在 [ **伺服器角色** ] 畫面上，按 **[下一步]** 。
6. 在 [ **功能** ] 畫面上，選取 **Windows Server Backup** ，然後按 **[下一步** 
    ![ 安裝備份]。](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. 按一下 [Install]  。
8. 安裝完成後，按一下 [ **關閉** ]。

### <a name="to-perform-a-backup-with-windows-server-backup"></a>使用 Windows Server Backup 執行備份

1. 開啟 **伺服器管理員** ，按一下 [ **工具** ]，然後按一下 [ **Windows Server Backup** ]。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，按一下 [ **開始** ]，指向 [系統 **管理工具** ]，然後按一下 [ **Windows Server Backup** ]。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. 如果出現提示，請在 [ **使用者帳戶控制** ] 對話方塊中，提供備份操作員認證，然後按一下 **[確定]** 。
3. 按一下 [ **本機備份** ]。
4. 在 [執行]  功能表上按一下 [一次性備份]  。
5. 在 [備份一次] 嚮導的 [ **備份選項** ] 頁面上，按一下 [ **不同選項** ]，然後按 **[下一步** ]。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. 在 [ **選取備份** 設定] 頁面上，按一下 [ **完整伺服器 (建議的)** ]，然後按 **[下一步]** 。
7. 在 [ **指定目的地類型** ] 頁面上，按一下 [ **本機磁片磁碟機** 或 **遠端共用資料夾** ]，然後按 **[下一步]** 。
8. 在 [ **選取備份目的地** ] 頁面上，選擇備份位置。  如果您選取 [本機磁片磁碟機]，請選擇 [本機磁片磁碟機]，或選取 [遠端共用] 選擇網路共用。
9. 在確認畫面上，按一下 [ **備份** ]。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. 完成之後，按一下 [ **關閉** ]。
11. 關閉 Windows Server Backup。

>[!NOTE]
>如果您收到錯誤，指出沒有可用的備份儲存位置，您必須排除其中一個已選取的磁片區，或新增磁片區或遠端共用。
>如果您收到一則警告，指出選取的磁片區也包含在要備份的專案清單中，請確定是否要移除然後按一下 **[確定]** 。

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>使用 Wbadmin.exe 來備份 windows server

Wbadmin.exe 是一種命令列公用程式，可讓您從命令提示字元備份和還原作業系統、磁片區、檔案、資料夾和應用程式。

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>使用 Wbadmin.exe 執行完整伺服器備份

- 開啟提升許可權的命令提示字元，輸入下列命令，然後按 ENTER 鍵：

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
