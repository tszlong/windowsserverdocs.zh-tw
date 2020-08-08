---
title: AD 樹系復原-備份系統狀態資料
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.openlocfilehash: 675b8edd5425238b838df2c2bb7e09d0c0232c92
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956885"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>AD 樹系復原-備份系統狀態資料

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用下列程式，使用 Windows Server Backup 或 wbadmin.exe 在 DC 上執行系統狀態備份。

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>若要使用 Windows Server Backup 執行系統狀態備份

1. 開啟**伺服器管理員**，按一下 [**工具**]，然後按一下 [ **Windows Server Backup**]。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，按一下 [**開始**]，指向 [系統**管理工具**]，然後按一下 [ **Windows Server Backup**]。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. 如果系統提示您，請在 [**使用者帳戶控制**] 對話方塊中，提供備份操作員認證，然後按一下 **[確定]**。
3. 按一下 [**本機備份**]。
4. 在 [執行]**** 功能表上按一下 [一次性備份]****。
5. 在 [一次性備份] 嚮導的 [**備份選項**] 頁面上，按一下 [**其他選項**]，然後按 **[下一步]**。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. 在 [**選取備份**設定] 頁面上，按一下 [**自訂) **]，然後按 **[下一步]**。
7. 在 [**選取備份的專案**] 畫面上，按一下 [**新增專案**] 並選取 [**系統狀態**]，然後按一下 **[確定]**。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，選取要包含在備份中的磁片區。 如果您選取 [**啟用系統修復**] 核取方塊，則會選取所有重要的磁片區。

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)

8. 按一下 [**指定目的地類型**] 頁面上的 [**本機磁片磁碟機**] 或 [**遠端共用資料夾**]，然後按 **[下一步]**。  如果您要備份至遠端共用資料夾，請執行下列動作：
   - 輸入共用資料夾的路徑。
   - 在 [**存取控制**] 底下，選取 [**不要繼承**或**繼承**] 來判斷備份的存取權，然後按 **[下一步]**。
   - 在 [**提供備份的使用者認證**] 對話方塊中，提供具有共用資料夾寫入權限之使用者的使用者名稱和密碼，然後按一下 **[確定]**。

9. 若為 Windows Server 2008 R2 和 Windows Server 2008，請在 [**指定 advanced 選項**] 頁面上，選取 [ **VSS 複本備份**]，然後按 **[下一步]**。
10. 在 [**選取備份目的地**] 頁面上，選擇備份位置。  如果您選取 [本機磁片磁碟機]，請選擇本機磁片磁碟機，或如果您選取 [遠端共用]，請選擇網路共用。
11. 在 [確認] 畫面上，按一下 [**備份**]。
12. 完成此作業之後，請按一下 [**關閉**]。
13. 關閉 Windows Server Backup。

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>若要使用 Wbadmin.exe 執行系統狀態備份

開啟提升許可權的命令提示字元，輸入下列命令，然後按 ENTER 鍵：

   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![安裝備份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
