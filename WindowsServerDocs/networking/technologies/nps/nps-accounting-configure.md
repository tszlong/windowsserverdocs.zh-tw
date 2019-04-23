---
title: 設定網路原則伺服器計量
description: 本主題提供文字檔案和記錄的 Windows Server 2016 中的網路原則伺服器的 SQL Server 的相關資訊。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: c732a9f42d942ad579468d1dd15d30324d6fea87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839699"
---
# <a name="configure-network-policy-server-accounting"></a>設定網路原則伺服器計量

有三種類型的網路原則伺服器記錄\(NPS\):

- **事件記錄**。 主要用於稽核和疑難排解連線嘗試。 您可以設定 NPS 事件記錄取得 NPS 內容，在 NPS 主控台中。

- **使用者驗證與帳戶處理要求記錄到本機檔案**。 主要用於連線分析及計費用途。 也很有用，做為安全性調查工具因為它會提供您追蹤活動的惡意使用者攻擊後方法。 您可以設定使用帳戶處理設定精靈 的本機檔案記錄。

- **使用者驗證與帳戶處理要求記錄到 Microsoft SQL Server 符合 XML 資料庫**。 用來允許執行 NPS，有一個資料來源的多部伺服器。 也提供使用關聯式資料庫的優點。 您可以使用帳戶處理設定精靈來設定 SQL Server 記錄。

## <a name="use-the-accounting-configuration-wizard"></a>使用帳戶處理設定精靈

使用帳戶處理設定精靈，您可以設定下列四個帳戶處理設定：

- **SQL 記錄只**。 藉由使用這項設定，您可以設定允許 NPS 連線至，並將計量資料傳送至 SQL server 的 SQL Server 的資料連結。 此外，精靈可以設定 SQL Server，以確保相容於 NPS SQL server 記錄資料庫上的資料庫。
- **僅文字記錄**。 藉由使用這項設定，您可以設定 NPS 帳戶資料記錄到文字檔。
- **平行記錄**。 藉由使用這項設定，您可以設定連結的 SQL Server 資料和資料庫。 您也可以設定文字檔案記錄，讓 NPS 將同時記錄至文字檔案和 SQL Server 資料庫。 
- **使用備份的 SQL 記錄**。 藉由使用這項設定，您可以設定連結的 SQL Server 資料和資料庫。 此外，您可以設定 NPS 使用 SQL Server 記錄失敗時的文字檔案記錄。

這些設定，除了 SQL Server 記錄檔和文字記錄可讓您指定 NPS 是否要繼續記錄失敗時處理連線要求。 您可以指定這**記錄失敗動作區段**本機檔案記錄內容，在 SQL server 記錄內容中，以及當您執行 [帳戶處理設定精靈] 中。

### <a name="to-run-the-accounting-configuration-wizard"></a>若要執行帳戶處理設定精靈

若要執行 [帳戶處理設定精靈]，完成下列步驟：

1. 開啟 NPS 主控台或 NPS Microsoft Management Console (MMC) 嵌入式管理單元。
2. 在主控台樹狀目錄中，按一下**Accounting**。
3. 在 [詳細資料] 窗格中，在**Accounting**，按一下**設定 Accounting**。

## <a name="configure-nps-log-file-properties"></a>設定 NPS 記錄檔內容

您可以設定網路原則伺服器 (NPS) 執行遠端驗證撥入使用者服務 (RADIUS) 帳戶處理的使用者驗證要求、 Access-accept 訊息、 Access-reject 訊息、 帳戶處理要求和回應，並定期狀態更新。 您可以使用此程序來設定您要存放計量資料記錄檔。

如需有關如何解譯記錄檔的詳細資訊，請參閱 <<c0> [ 解譯 NPS 資料庫格式記錄檔](https://technet.microsoft.com/library/cc771748.aspx)。

若要避免記錄檔填滿硬碟，強烈建議您將它們保存在磁碟分割以外的系統磁碟分割中。 以下提供設定 NPS 帳戶處理的詳細資訊：

- 若要傳送另一個處理序收集記錄檔資料，您可以設定 NPS 寫入具名管道。 若要使用具名的管道、 設定記錄檔資料夾\\。 \pipe 或\\ComputerName\pipe。 具名的管道伺服器程式會建立稱為 「 具名的管道\\.\pipe\iaslog.log 接受資料。 在 本機檔案內容 對話方塊中，在 建立新的記錄檔，永遠不會選取 （不限制的檔案大小） 當您使用具名管道。

- 可以在系統環境變數 （而不是使用者變數），例如 %systemdrive%、 %systemroot%和 %windir%中建立的記錄檔目錄。 例如，下列路徑，使用環境變數 %windir%，找出系統目錄的記錄檔中子資料夾 \System32\Logs (也就是 %windir%\System32\Logs\)。

- 切換記錄檔格式，不會造成要建立新的記錄檔。 如果您變更記錄檔格式，在作用中變更的時間的檔案將包含混合的兩種格式 （記錄在記錄檔開始處會有先前的格式，並記錄在記錄檔結尾處會有新的格式）。

- 如果 RADIUS 帳戶處理失敗，因為完整的硬碟磁碟機或其他原因，NPS 會停止處理連線要求，防止使用者存取網路資源。

- NPS 可記錄到 Microsoft® SQL Server™ 資料庫，或 instead of 記錄到本機檔案。

中的成員資格**Domain Admins**群組是要執行此程序，至少需要。


### <a name="to-configure-nps-log-file-properties"></a>若要設定 NPS 記錄檔內容

1. 開啟 NPS 主控台或 NPS Microsoft Management Console (MMC) 嵌入式管理單元。
2. 在主控台樹狀目錄中，按一下**Accounting**。
3. 在 [詳細資料] 窗格中，在**記錄檔內容**，按一下**變更記錄檔內容**。 **記錄檔內容**對話方塊隨即開啟。
4. 在 **記錄檔內容**上**設定**索引標籤**記錄下列資訊**，請確定您選擇要記錄足夠的資訊，以達到您的帳戶處理目標。 例如，如果您的記錄檔需要完成的工作階段相互關聯，請選取所有核取方塊。
5. 在 **失敗動作記錄**，選取**記錄失敗時，捨棄連線要求**如果想要 NPS 時停止處理 Access-request 訊息記錄檔已滿或無法使用基於某些原因。 如果您想要 NPS 以繼續處理連線要求，記錄失敗時，請勿選取此核取方塊。
6. 在 **記錄檔內容** 對話方塊中，按一下**記錄檔** 索引標籤。
7. 在上**記錄檔**索引標籤中，於**Directory**，輸入您要存放 NPS 記錄檔的位置。 預設位置是 systemroot\System32\LogFiles 資料夾。<br>如果您未提供的完整路徑陳述式**記錄檔目錄**，會使用預設路徑。 例如，如果您鍵入**NPSLogFile**中**記錄檔目錄**，檔案位於 %systemroot%\system32\npslogfile。
8. 在 **格式**，按一下**DTS 相容**。 如果您想，您可以改為選取舊版檔案格式，例如**ODBC\(舊版\)** 或是**IAS\(舊版\)**。<br>**ODBC**並**IAS**舊版的檔案類型包含的 NPS 傳送到其 SQL Server 資料庫的資訊子集。 **DTS 相容**檔案類型的 XML 格式完全相同，NPS 會使用其 SQL Server 資料庫將資料匯入的 XML 格式。 因此， **DTS 相容**檔案格式會提供更有效率且完整的資料傳輸到標準的 SQL Server 資料庫的 NPS。
9. 在 **建立新的記錄檔**，若要設定 NPS 以啟動新的記錄檔，以指定的間隔，按一下您想要使用的間隔：
    - 對於交易繁忙磁碟區與記錄活動，按一下**每日**。
    - 對於較小的交易量與記錄活動，按一下**每週**或是**每月**。
    - 若要將所有交易都儲存在一個記錄檔中，按一下**永不\(無限制的檔案大小\)**。
    - 若要限制每個記錄檔的大小，請按一下**記錄檔達到此大小時**，然後輸入檔案大小之後, 建立新的記錄。 預設大小是 10 mb (MB)。
10. 如果您想要 NPS 在刪除舊的記錄檔，以建立新的記錄檔的磁碟空間接近容量的硬碟時，請確認**當磁碟已完全刪除舊的記錄檔**已選取。 此選項不提供，不過，如果值**建立新的記錄檔**是**永不\(不限制的檔案大小\)**。 此外，如果最舊的記錄檔是目前的記錄檔，它不會刪除。

## <a name="configure-nps-sql-server-logging"></a>設定 NPS SQL 伺服器記錄

您可以使用此程序至執行 Microsoft SQL Server 的本機或遠端資料庫的記錄檔 RADIUS 帳戶處理資料。

>[!NOTE]
>NPS 帳戶處理資料格式化為 XML 文件傳送至**report_event**預存程序，在您指定在 NPS 中的 SQL Server 資料庫中。 SQL Server 記錄正常運作，您必須具有名為預存程序**report_event** SQL Server 資料庫來接收和剖析來自 NPS 的 XML 文件中。

要完成此程序，至少需要的成員資格 Domain Admins 或同等權限。

### <a name="to-configure-sql-server-logging-in-nps"></a>若要設定 NPS 中的記錄的 SQL Server

1. 開啟 NPS 主控台或 NPS Microsoft Management Console (MMC) 嵌入式管理單元。
2. 在主控台樹狀目錄中，按一下**Accounting**。
3. 在 [詳細資料] 窗格中，在**SQL Server 記錄內容**，按一下**變更 SQL Server 記錄內容**。 **SQL Server 記錄內容**對話方塊隨即開啟。
4. 在 **記錄下列資訊**，選取您想要記錄的資訊： 
    - 若要記錄所有帳戶處理要求，按一下**帳戶處理要求**。
    - 若要記錄驗證要求，請按一下**驗證要求**。
    - 若要記錄定期帳戶處理狀態，按一下**定期帳戶處理狀態**。
    - 若要記錄定期狀態，例如暫時帳戶處理要求，按一下**定期狀態**。
5. 若要設定執行 NPS 和 SQL Server 的伺服器之間所允許的並行工作階段數目，輸入中的數字**並行工作階段的最大數目**。
6. 若要設定 SQL Server 資料來源，在**SQL Server 記錄**，按一下**設定**。 **資料連結屬性**對話方塊隨即開啟。 在 **連線**索引標籤上，指定下列項目： 
    - 若要指定儲存資料庫的伺服器名稱，輸入或選取中的名稱**選取或輸入伺服器名稱**。
    - 若要指定用來登入伺服器的驗證方法，請按一下**使用 Windows NT 整合式安全性**。 或者，按一下**使用特定的使用者名稱和密碼**，然後輸入中的認證**使用者名稱**並**密碼**。
    - 若要允許空白密碼，請按一下**空白密碼**。
    - 若要儲存的密碼，按一下**允許儲存密碼**。
    - 若要指定要連接到執行 SQL Server 的電腦上的資料庫，請按一下**選取 在伺服器上的資料庫**，然後從清單中選取 資料庫名稱。
7. 若要測試 NPS 與 SQL Server 之間的連線，請按一下**測試連接**。 按一下  **確定**以關閉**資料連結屬性**。
8. 在 **失敗動作記錄**，選取**啟用容錯移轉的文字檔案記錄**是否要讓 NPS 在文字檔案記錄 SQL Server 記錄失敗時繼續執行。 
9. 在 **失敗動作記錄**，選取**記錄失敗時，捨棄連線要求**如果想要 NPS 時停止處理 Access-request 訊息記錄檔已滿或無法使用基於某些原因。 如果您想要 NPS 以繼續處理連線要求，記錄失敗時，請勿選取此核取方塊。

## <a name="ping-user-name"></a>Ping 使用者名稱

有些 RADIUS proxy 伺服器與網路存取伺服器定期傳送 （又稱為 ping 要求） 的驗證和帳戶處理要求，以確認 NPS 在網路上。 這些 ping 要求包括虛構的使用者名稱。 當 NPS 處理這些要求時，存取拒絕記錄，因此更難以追蹤有效記錄的事件與帳戶處理記錄檔變得填滿。

當您設定的登錄項目**ping 使用者名稱**，NPS 會比對 ping 要求中的使用者名稱值的登錄項目值對其他伺服器。 A **ping 使用者名稱**登錄項目會指定 RADIUS proxy 伺服器與網路存取伺服器所傳送的虛構使用者名稱 （或使用者名稱，變數使用，符合模式的虛構使用者名稱）。 當 NPS 收到符合的 ping 要求**ping 使用者名稱**登錄項目值，NPS 會拒絕驗證要求而不需處理要求。 NPS 不會記錄交易涉及的虛構的使用者名稱中任何記錄檔，讓您更輕鬆地解譯事件記錄檔。

**Ping 使用者名稱**預設並未安裝。 您必須新增**ping 使用者名稱**至登錄。 您可以使用登錄編輯程式登錄新增項目。

>[!CAUTION]
>不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

### <a name="to-add-ping-user-name-to-the-registry"></a>將 ping 使用者名稱新增至登錄

Ping 使用者名稱可以加入下列登錄機碼的字串值的本機 Administrators 群組成員：

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **名稱**: `ping user-name`
- **型別**: `REG_SZ`
- **資料**:*使用者名稱*

>[!TIP]
>若要表示多個使用者名稱**ping 使用者名稱**值，請輸入名稱模式，例如 DNS 名稱，包括中的萬用字元**資料**。
