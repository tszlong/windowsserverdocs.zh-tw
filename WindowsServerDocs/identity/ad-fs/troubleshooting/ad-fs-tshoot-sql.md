---
title: AD FS 疑難排解-SQL 連線能力
description: 瞭解如何針對 SQL 連線能力方面 AD FS 的各種層面進行疑難排解。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.openlocfilehash: 0d68e71c4ac4d1e3d03bd3c549ccc6f82e68ebe5
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965800"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>AD FS 疑難排解-SQL 連線能力
AD FS 可讓您針對 AD FS 伺服器陣列資料使用遠端 SQL Server。  如果伺服器陣列中的 AD FS 伺服器無法與後端 SQL server 通訊，您將會看到問題。  下列檔將提供一些基本步驟來測試與後端伺服器的通訊。

## <a name="acquire-the-sql-database-connection-string"></a>取得 SQL database 連接字串
檢查 SQL 連線能力時要測試的第一件事是，如果 AD FS 有正確的 SQL 連接資訊，則為。  這可以使用 PowerShell 來完成。

### <a name="to-acquire-the-sql-connection-string"></a>取得 SQL 連接字串
1.  開啟 Windows PowerShell
2. 輸入下列內容： `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` 並按 Enter 鍵
3. 輸入下列內容：  `$adfs.ConfigurationDatabaseConnectionString` 並按 enter 鍵。
4. 您應該會看到連接字串資訊。

![PowerShell 命令畫面正在執行命令](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>建立 (UDL) 檔案的通用資料連結以測試連線能力
通用資料連結檔案或 UDL 檔案基本上是包含資料庫連接字串的文字檔。  藉由使用我們在上面取得的資訊，我們可以測試 SQL server 是否會回應連接。

### <a name="to-create-a-testudl-file-to-test-connectivity"></a>建立測試 udl 檔案以測試連線能力

1. 開啟 [記事本]，並將檔案儲存為 [測試]。  確定您已從 [**存檔類型**] 的下拉式清單中選取 **所有** 檔案。
2. 按兩下 [測試]
3. 填入下列資訊：。 **選取或輸入伺服器名稱：**  使用上述連接字串中的資料來源。 **輸入資訊以登入伺服器：**  使用 AD FS 服務帳戶，或具有遠端登入權利的帳戶。  如果帳戶是 windows 帳戶，請使用整合式驗證，否則請輸入使用者名稱和密碼。
    c. **選取伺服器上的資料庫：** 使用上述字串的初始目錄。  範例： AdfsConfigurationV3。
   ![測試連接](media/ad-fs-tshoot-sql/sql4.png)
1. 按一下 **[測試連接]** 。</br>
「成功」![](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>使用 SQL Server Management Studio 來測試連線能力
您也可以 [下載](https://go.microsoft.com/fwlink/?linkid=864329) 並安裝 SSMS 以測試資料庫連線能力。

### <a name="to-test-connectivity-with-ssms"></a>使用 SSMS 測試連線能力
1. 下載並安裝 SQL Server Management Studio。
![安裝](media/ad-fs-tshoot-sql/sql5.png)
1. 開啟 SSMS，然後輸入伺服器名稱。  上述的資料來源。
2. 使用 AD FS 服務帳戶，或具有遠端登入權利的帳戶。  如果帳戶是 windows 帳戶，請使用整合式驗證，否則請輸入使用者名稱和密碼。
![[連接]](media/ad-fs-tshoot-sql/sql6.png)
1. 您應該會看到左方填入的位置。  展開 [資料庫]，並確認您看到 AD FS 資料庫。
![AD FS 資料庫](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)