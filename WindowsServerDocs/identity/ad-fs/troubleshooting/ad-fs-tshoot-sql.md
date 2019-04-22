---
title: AD FS 疑難排解-SQL 連線能力
description: 本文件說明如何疑難排解各個層面的 AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a4b7a568200bee7c2696c57f1dd964dd4e84ec21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820259"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>AD FS 疑難排解-SQL 連線能力
AD FS 提供使用遠端 SQL Server 的 AD FS 伺服器陣列資料的能力。  如果您的伺服器陣列中的 AD FS 伺服器無法與後端 SQL server 通訊，您會看到問題。  下列文件將提供一些基本的步驟，來測試與後端伺服器之間的通訊。

## <a name="acquire-the-sql-database-connection-string"></a>取得 SQL database 連接字串
測試時檢查 SQL 連接性的第一件事是，如果 AD FS 具有正確的 SQL 連線資訊。  這可以使用 PowerShell。

### <a name="to-acquire-the-sql-connection-string"></a>若要取得的 SQL 連接字串
1.  開啟 Windows PowerShell
2. 輸入下列命令：`$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService`按 enter 鍵
3. 輸入下列命令：`$adfs.ConfigurationDatabaseConnectionString`然後按 enter 鍵。
4. 您應該會看到的連接字串資訊。
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>建立通用資料連結 (UDL) 檔案來測試連線能力
通用資料連結檔案或 UDL 檔案基本上是一個文字檔，其中包含資料庫連接字串。  使用上面所取得的資訊中，我們可以測試在 SQL server 正在回應的連線。

### <a name="to-create-a-udl-file-to-test-connectivity"></a>若要建立 udl 檔案來測試連線

1. 開啟 [記事本]，並將檔案儲存為 test.udl。  請確定您已**所有的檔案**從下拉式清單中選取**存檔類型**。
2. 按兩下 test.udl
3. 填寫下列資訊：。 **選取或輸入伺服器名稱：** 使用資料來源，從 b 上方的連接字串。 **輸入要登入伺服器的資訊：** 使用 AD FS 服務帳戶或可從遠端登入的權限的帳戶。  如果帳戶是 windows 帳戶使用整合式驗證，否則會輸入使用者名稱和密碼。
    c.  **選取的伺服器上的資料庫：** 使用初始目錄從上面的字串。  範例：AdfsConfigurationV3.
   ![測試連接](media/ad-fs-tshoot-sql/sql4.png)
1. 按一下 **測試連接**。</br>
![Success](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>使用 SQL Server Management Studio 來測試連線能力
您也可以[下載](https://go.microsoft.com/fwlink/?linkid=864329)並安裝 SSMS 來測試資料庫連線能力。

###<a name="to-test-connectivity-with-ssms"></a>若要測試使用 SSMS 連線
1. 下載並安裝 SQL Server Management Studio。
![安裝](media/ad-fs-tshoot-sql/sql5.png)
1. 開啟 SSMS 中，輸入伺服器名稱。  從上方的資料來源。
2. 使用 AD FS 服務帳戶或可從遠端登入的權限的帳戶。  如果帳戶是 windows 帳戶使用整合式驗證，否則會輸入使用者名稱和密碼。
![Connect](media/ad-fs-tshoot-sql/sql6.png)
1. 您應該會看到左側填入。  展開資料庫，並確認您看到 AD FS 資料庫。
![AD FS 資料庫](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)