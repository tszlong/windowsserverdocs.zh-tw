---
title: 設定的網路原則伺服器計量
description: 本主題提供文字檔案和 SQL Server 登入 Windows Server 2016 中的網路原則伺服器的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69341e30d90ee1be29c40d835a4f71fe433c11dc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policy-server-accounting"></a>設定的網路原則伺服器計量

有三種類型的網路原則伺服器 \(NPS\) 登入：

- **事件登入**。 主要用於稽核和連接嘗試進行疑難排解。 您可以設定 NPS 事件取得 NPS 伺服器屬性 NPS 主機登入。

- **登入本機的檔案的驗證使用者以及計量要求**。 主要用來連接分析及計費用途。 也很有用，做為安全性調查工具因為您提供攻擊後追蹤惡意使用者的活動的方法。 您可以設定使用計量組態精靈本機檔案登入。

- **登入的 Microsoft SQL Server 相容 XML 資料庫驗證使用者以及計量要求**。 用來讓多部伺服器執行 NPS 有一個資料來源。 也提供使用的關係資料庫的優點。 您可以使用 [計量組態精靈設定 SQL Server 登入。

## <a name="use-the-accounting-configuration-wizard"></a>使用計量組態精靈

藉由使用計量設定精靈中，您可以設定下列四個計量設定：

- **僅限 SQL 登入**。 使用此設定，您可以設定可連接至並計量資料傳送至 SQL server NPS SQL Server 的資料的連結。 此外，精靈可以資料庫設定 SQL Server 以確保資料庫相容具有 NPS SQL server 登入。
- **僅限文字登入**。 使用此設定，您可以設定 NPS 登入計量資料文字檔案。
- **平行登入**。 使用此設定，您可以設定的資料 \] 連結 SQL Server 和資料庫。 您也可以設定文字檔案登入，以便 NPS 登同時文字檔案和 SQL Server 資料庫。 
- **備份與 SQL 登入**。 使用此設定，您可以設定的資料 \] 連結 SQL Server 和資料庫。 此外，您可以設定 NPS 使用 SQL Server 登入失敗時，文字檔案登入。

除了這些設定，請同時 SQL Server 登入和文字登入可讓您指定 NPS 是否要繼續處理連接要求如果登入失敗。 您也可以在此指定**登入失敗動作] 區段**在本機檔案登入屬性，SQL server 登入屬性，以及當您正在執行計量設定精靈。

### <a name="to-run-the-accounting-configuration-wizard"></a>若要執行計量設定精靈

若要執行計量設定精靈中，請完成下列步驟：

1. NPS 主機或 NPS Microsoft Management Console (MMC) 嵌入式管理單元開放。
2. 主控台中，按一下 [**計量**。
3. 在詳細資料窗格中，在**計量**，按一下 [**設定計量**。

## <a name="configure-nps-log-file-properties"></a>設定 NPS 登入檔案屬性

您可以設定的網路原則伺服器 (NPS) 到執行遠端驗證 Dial 使用者服務 (RADIUS) 計量使用者驗證要求，存取接受訊息、存取-退回訊息、計量要求和回應，以及狀態定期的更新。 若要設定您要儲存計量資料登入檔案，您可以使用此程序。

如需有關解譯登入檔案，請查看[上尚未取得共識檔案 NPS 資料庫格式登入](https://technet.microsoft.com/library/cc771748.aspx)。

若要防止填滿硬碟登入檔案，建議您將它們保存在不同的系統磁碟分割的磁碟分割。 以下提供設定計量 NPS 的相關資訊：

- 若要傳送檔案的登入資料收集其他處理程序，您可以設定 NPS 寫入命名管道。 若要使用管道命名，設定 \\.\pipe 或 \\ComputerName\pipe 登入檔案的資料夾。 命名的管道伺服器程式建立命名的管道稱為 \\.\pipe\iaslog.log 接受資料。 本機檔案屬性對話方塊中，在 [建立新的登入檔案，不會（無限制的檔案的大小）選取當您使用名的管道。

- 登入檔案 directory 可以使用系統環境變數（而不是使用者變數），例如 %系統磁碟機、systemroot %及 %windir%建立。 例如，下列路徑，使用環境變數 %windir%，找出登入檔案，系統 directory 在子資料夾 \System32\Logs (也就是 %windir%\system32\logs\)。

- 切換登入的檔案格式不會建立新的登入。 如果變更登入的檔案格式，變更的時間位於使用中的檔案將會包含兩種格式多種（記錄登入的開頭有先前格式，且記錄結尾的登入新的格式）。

- 如果 RADIUS 計量失敗因為完整硬碟或其他原因，NPS 停止處理連接要求，導致使用者存取網路資源。

- NPS 提供的能力來登入 Microsoft® SQL Server™ 資料庫而不是，或登入至本機的檔案。

資格在**網域系統管理員**群組是的最低需求才能執行此程序。


### <a name="to-configure-nps-log-file-properties"></a>若要設定 NPS 登入檔案屬性

1. NPS 主機或 NPS Microsoft Management Console (MMC) 嵌入式管理單元開放。
2. 主控台中，按一下 [**計量**。
3. 在詳細資料窗格中，在**登入檔案屬性**，按一下 [**變更登入檔案屬性**。 **登入檔案屬性**對話方塊。
4. 在**登入檔案屬性**，在**設定**索引標籤**登入下列資訊**，確認您選擇登入資訊不足，無法達到計量目標。 例如，若您登完成工作階段相關，請選取所有核取方塊。
5. 在**登入失敗動作**、**如果登入失敗時，會捨棄連接要求**如果您想要停止處理要求存取訊息時登入的檔案完全或無法使用某些原因 NPS。 如果您想繼續處理連接要求如果登入失敗 NPS，不會選取此核取方塊。
6. 在**登入檔案屬性**對話方塊中，按**登入檔案**索引標籤。
7. 在**設定檔登入**索引標籤的**Directory**，輸入您想要儲存 NPS 登入檔案的位置。 預設位置為 systemroot\System32\LogFiles 資料夾。
    >[!NOTE]
    >如果您不提供完整路徑陳述在**登入檔案 Directory**，使用預設的路徑。 例如，如果您輸入**NPSLogFile**在**登入檔案 Directory**，位於 %systemroot%\system32\npslogfile 檔案。
8. 在**格式**，按一下 [ **DTS 相容**。 如果您想要的話，您可以改為選取的舊版檔案格式，例如**ODBC \(Legacy\)**或**IAS \(Legacy\)**。
    >[!NOTE]
    >**ODBC**與**IAS**的舊版檔案類型包含子集 NPS 將傳送至其 edition 的資訊。 **相容 DTS**是 NPS 使用資料匯入到其 edition XML 格式相同的檔案類型 XML 格式。 因此，**相容 DTS**的檔案格式 NPS 提供更有效率且完整的資料傳送到標準 SQL Server 資料庫。
9. 在**建立新的檔案登入**，若要設定新的登入檔案，指定的時間間隔，[開始] 中，按一下您想要使用的時間間隔 NPS:
    - 對於大量交易音量與登入活動中，按一下**每天**。
    - 較少交易磁碟區及登入的活動，按**週**或**每月**。
    - 若要儲存的所有交易登入一個檔案，請按一下**永不 \(unlimited file size\)**。
    - 若要限制登入的每個檔案的大小，請按一下 [**登入檔大小的到達時**，然後輸入較檔案的大小，建立新的登入之後。 預設的大小是 10 mb。
10. 如果您想 NPS delete 舊的登入檔案，以建立新的登入檔案的磁碟空間容量硬碟時，請確定**磁碟時完整 delete 較舊的登入檔案**選取。 不此選項，不過，如果的值**建立新的檔案登入**是**永不 \(unlimited file size\)**。 同時，如果登入舊檔案目前登入檔案，它並不刪除。

## <a name="configure-nps-sql-server-logging"></a>NPS SQL Server 登入的設定

您可以使用此程序來登入 RADIUS 計量資料到 Microsoft SQL server 本機或遠端資料庫。

>[!NOTE]
>NPS 格式計量資料的 XML 文件，並將傳送至為**report_event**您在 NPS 指定 SQL Server 資料庫中儲存程序。 登入，才能正常 SQL Server 中，您必須有儲存程序名為**report_event**可以收到和剖析 NPS 的 XML 文件 SQL Server 資料庫中。

資格網域系統管理員」，或等最小，才能完成此程序。

### <a name="to-configure-sql-server-logging-in-nps"></a>若要設定登入 NPS SQL Server

1. NPS 主機或 NPS Microsoft Management Console (MMC) 嵌入式管理單元開放。
2. 主控台中，按一下 [**計量**。
3. 在詳細資料窗格中，在**SQL Server 登入屬性**，按一下 [**變更 SQL Server 登入屬性**。 **SQL Server 登入屬性**對話方塊。
4. 在**下列資訊的登入**，選取您想要登入資訊： 
    - 若要登入的所有計量要求，按一下 [**計量要求**。
    - 驗證要求登入，請按一下**驗證要求**。
    - 若要登入定期計量狀態，按一下 [**定期計量狀態**。
    - 若要登入定期狀態，例如暫時計量要求，按一下 [**定期狀態**。
5. 若要設定的之間執行 NPS 及 SQL Server 的伺服器允許同時工作階段，請輸入數字**最大值數字同時工作階段的**。
6. 在 [設定 SQL Server 資料來源，**SQL Server 登入**，按一下 [**設定**。 **資料連結屬性**對話方塊。 在**連接**索引標籤上，指定動作： 
    - 若要指定儲存資料庫伺服器的名稱，請輸入，或選取名稱**選取或輸入伺服器名稱**。
    - 若要指定要用來登入伺服器的驗證方法，請按一下**使用 NT 整合式的安全性**。 或按一下 [**使用特定的使用者名稱和密碼**，然後輸入認證中的**的使用者名稱**和**密碼**。
    - 若要允許空白的密碼，請按一下**空白密碼**。
    - 若要儲存的密碼，請按一下**允許將密碼儲存**。
    - 若要指定要資料庫連接到執行 SQL Server 的電腦上，按一下 [**選取伺服器上的資料庫**，然後從清單選取資料庫的名稱。
7. 若要測試 NPS 和 SQL Server 之間的連接，請按一下**測試連接**。 按一下**[確定]**以關閉 [**資料連結屬性**。
8. 在**登入失敗動作**，請選取**讓文字檔案登入容錯移轉的**如果您想要 NPS 繼續文字檔案登入如果 SQL Server 登入失敗。 
9. 在**登入失敗動作**、**如果登入失敗時，會捨棄連接要求**如果您想要停止處理要求存取訊息時登入的檔案完全或無法使用某些原因 NPS。 如果您想繼續處理連接要求如果登入失敗 NPS，不會選取此核取方塊。

## <a name="ping-user-name"></a>Ping 使用者名稱

某些 RADIUS proxy 伺服器和網路存取伺服器定期驗證 NPS 伺服器會顯示在網路上傳送驗證及計量要求（稱為 ping 要求）。 這些 ping 要求包含虛構使用者名稱。 NPS 處理這些要求時，存取退回記錄，讓它更難來保留有效記錄成為填滿事件及計量登。

當您設定登錄項目適用於**ping 使用者名稱**、NPS 其他伺服器符合 ping 要求的使用者名稱值對登錄項目值。 A **ping 使用者名稱**登錄指定虛構使用者名稱（或使用者名稱模式，變數，以符合虛構使用者名稱）傳送 RADIUS proxy 伺服器或網路存取伺服器。 NPS 時收到 ping 要求符合**ping 使用者名稱**登錄項目值、NPS 拒絕驗證要求處理，此要求。 NPS 記錄交易涉及虛構中的使用者名稱任何登入檔案，讓事件登入變得更容易上尚未取得共識。

**Ping 使用者名稱**預設不會安裝。 您必須將**ping 使用者名稱**登錄。 您可以使用作業系統登錄新增項目。

>[!CAUTION]
>編輯登錄錯誤嚴重可能會損壞您的系統。 變更登錄以前，您應該備份在電腦上的任何重要的資料。

### <a name="to-add-ping-user-name-to-the-registry"></a>若要新增 ping 使用者名稱登錄

Ping 使用者名稱新增下列機碼至字串值的系統管理員本機群組成員：

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **名稱**: `ping user-name`
- **輸入**: `REG_SZ`
- **資料**:*的使用者名稱*

>[!TIP]
>若要指出多個使用者名稱為**ping 使用者名稱**值，輸入名稱模式，包括萬用字元，在 [DNS 名稱，例如**資料**。
