---
title: 先進的疑難排解伺服器訊息區（SMB）
description: 引進先進伺服器訊息區（SMB）的疑難排解方法。
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 21b090e8e70f287e9609d28588403e3aa0988ce4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966300"
---
# <a name="advanced-troubleshooting-server-message-block-smb"></a>先進的疑難排解伺服器訊息區（SMB）

伺服器訊息區（SMB）是一種用於檔案系統作業的網路傳輸通訊協定，可讓用戶端存取伺服器上的資源。 SMB 通訊協定的主要目的是要透過 TCP/IP 來啟用兩個系統之間的遠端檔案系統存取。

SMB 疑難排解可能非常複雜。 本文並不是詳盡的疑難排解指南，而是瞭解如何有效疑難排解 SMB 的基本知識，這是一個簡短的入門。

## <a name="tools-and-data-collection"></a>工具和資料收集

高品質 SMB 疑難排解的一個重要層面是溝通正確的詞彙。 因此，本文會介紹基本的 SMB 術語，以確保資料收集和分析的正確性。

> [!Note]
> *SMB 伺服器（SRV）* 指的是裝載檔案系統（也稱為檔案伺服器）的系統。 *SMB 用戶端（CLI）* 指的是嘗試存取檔案系統的系統，不論作業系統版本或版本為何。

例如，如果您使用 Windows Server 2016 來連線到裝載在 Windows 10 上的 SMB 共用，Windows Server 2016 就是 smb 用戶端和 Windows 10 SMB 伺服器。

### <a name="collect-data"></a>收集資料

在您針對 SMB 問題進行疑難排解之前，建議您先在用戶端和伺服器端收集網路追蹤。 以下是適用的方針：

- 在 Windows 系統上，您可以使用 [netshell （netsh）]、[網路監視器]、[Message 分析器] 或 [Wireshark] 來收集網路追蹤。

- 協力廠商裝置通常具有內建的封包捕捉工具，例如 tcpdump （Linux/FreeBSD/Unix）或 pktt （NetApp）。 例如，如果 SMB 用戶端或 SMB 伺服器是 Unix 主機，您可以執行下列命令來收集資料：
  
  ```cmd
  # tcpdump -s0 -n -i any -w /tmp/$(hostname)-smbtrace.pcap
  ```
  
  使用鍵盤上的**Ctrl + C**停止收集資料。

若要探索問題的來源，您可以檢查雙面追蹤： CLI、SRV 或其間的某個位置。

#### <a name="using-netshell-to-collect-data"></a>使用 netshell 收集資料

本節提供使用 netshell 收集網路追蹤的步驟。

> [!NOTE]  
> Netsh 追蹤會建立 ETL 檔案。 ETL 檔案只能在 Message Analyzer （MA）和網路監視器3.4 中開啟（將剖析器設定為網路監視器剖析器 \> 視窗）。

1. 在 SMB 伺服器和 SMB 用戶端上，于磁片磁碟機**C**上建立**暫存**資料夾。然後，執行下列命令：

   ```cmd
   netsh trace start capture=yes report=yes scenario=NetConnection level=5 maxsize=1024 tracefile=c:\\Temp\\%computername%\_nettrace.etl**
   ```
   
   如果您使用 PowerShell，請執行下列 Cmdlet：
   
   ```PowerShell
   New-NetEventSession -Name trace -LocalFilePath "C:\Temp\$env:computername`_netCap.etl" -MaxFileSize 1024

   Add-NetEventPacketCaptureProvider -SessionName trace -TruncationLength 1500

   Start-NetEventSession trace
   ```
   
2. 重現問題。

3. 執行下列命令來停止追蹤：

   ```cmd
   netsh trace stop
   ```
   
   如果您使用 PowerShell，請執行下列 Cmdlet：

   ```PowerShell
   Stop-NetEventSession trace  
   Remove-NetEventSession trace
   ```

> [!NOTE] 
> 您應該只追蹤所傳輸的資料量下限。 針對效能問題，如果情況允許，一定會同時採取良好和不良的追蹤。

### <a name="analyze-the-traffic"></a>分析流量

SMB 是應用層級的通訊協定，使用 TCP/IP 做為網路傳輸通訊協定。 因此，SMB 問題也可能是由 TCP/IP 問題所造成。

檢查 TCP/IP 是否會遇到下列任何問題：

1. TCP 三向交握未完成。 這通常表示有防火牆區塊，或伺服器服務不在執行中。

2. 發生重新傳輸。 這些可能會因為複合 TCP 擁塞節流而導致檔案傳送速度變慢。

3. 5次重新傳輸後再進行 TCP 重設，可能表示系統之間的連線遺失，或其中一個 SMB 服務當機或停止回應。

4. TCP 接收視窗會降低。 這可能是因為儲存速度緩慢或一些其他的問題，而導致無法從輔助函數驅動程式（AFD） Winsock 緩衝區抓取資料。

如果沒有明顯的 TCP/IP 問題，請尋找 SMB 錯誤。 若要這樣做，請遵循下列步驟：

1. 請一律檢查 SMB 錯誤是否符合 MS SMB2 通訊協定規格。 許多 SMB 錯誤都是良性的（不有害）。 請參閱下列資訊，以判斷在您結束錯誤與下列任何問題相關之前，SMB 為何會傳回錯誤：

   - [SMB2 訊息語法](/openspecs/windows_protocols/ms-smb2/6eaf6e75-9c23-4eda-be99-c9223c60b181)主題會詳細說明每個 SMB 命令和其選項。
    
   - [MS SMB2 用戶端處理](/openspecs/windows_protocols/ms-smb2/df0625a5-6516-4fbe-bf97-01bef451cab2)主題會詳細說明 SMB 用戶端如何建立要求並回應伺服器消息。

   - [MS SMB2 伺服器處理](/openspecs/windows_protocols/ms-smb2/e1d08834-42e0-41ca-a833-fc26f5132a6f)主題會詳細說明 SMB 伺服器如何建立要求並回應用戶端要求。

2. 檢查是否會在 FSCTL \_ 驗證 \_ negotiate \_ 資訊（驗證 negotiate）命令之後立即傳送 TCP 重設命令。 若是如此，請參閱下列資訊：

   - 當用戶端或伺服器上的 [驗證] 協調流程失敗時，SMB 會話必須終止（TCP 重設）。

   - 此進程可能會失敗，因為 WAN 優化工具正在修改 SMB Negotiate 封包。

   - 如果連接過早結束，請識別用戶端與伺服器之間的最後一次 exchange 通訊。

#### <a name="analyze-the-protocol"></a>分析通訊協定

查看網路追蹤中的實際 SMB 通訊協定詳細資料，以瞭解所使用的確切命令和選項。

> [!NOTE]
> 只有 Message Analyzer 可以剖析 SMBv3 和更新版本的命令。

- 請記住，SMB 只會執行它所告知的動作。

- 藉由檢查 SMB 命令，您可以深入瞭解應用程式嘗試執行的工作。

將命令和作業與通訊協定規格進行比較，以確保所有專案都能正常運作。 如果不是，請收集更接近或較低層級的資料，以尋找有關根本原因的詳細資訊。 若要這樣做，請遵循下列步驟：

1. 收集標準封包捕獲。

2. 執行**netsh**命令，追蹤並收集有關網路堆疊中是否有問題或 Windows 篩選平台（WFP）應用程式（例如防火牆或防毒程式）的詳細資料。

3. 如果所有其他選項都失敗，請在您懷疑問題是在 SMB 本身內發生，或是沒有其他資料足以識別根本原因時，收集 t-sql。

例如：

- 對單一檔案伺服器的檔案傳輸速度變慢。

- 雙面追蹤顯示 SRV 回應讀取要求的速度很慢。

- 移除防毒程式會解析緩慢的檔案傳輸。

- 您必須聯絡防毒程式 manufactory 以解決問題。

> [!NOTE]
> （選擇性）您**也**可以在疑難排解期間暫時卸載防毒程式。

#### <a name="event-logs"></a>事件記錄檔

SMB 用戶端和 SMB 伺服器都有詳細的事件記錄檔結構，如下列螢幕擷取畫面所示。 收集事件記錄檔，以協助找出問題的根本原因。

![事件記錄檔](media/troubleshooting-smb-1.png)

## <a name="smb-related-system-files"></a>SMB 相關的系統檔案

此區段會列出 SMB 相關的系統檔案。 若要讓系統檔案保持更新，請確定已安裝最新的[更新彙總套件](https://support.microsoft.com/help/4498140/windows-10-update-history)。

在 **% windir% \\ system32 \\ 驅動程式**底下列出的 SMB 用戶端二進位檔：

- RDBSS.sys

- MRXSMB.sys

- MRXSMB10.sys

- MRXSMB20.sys

- MUP.sys

- SMBdirect.sys

在 **% windir% \\ system32 \\ 驅動程式**底下列出的 SMB 伺服器二進位檔：

- SRVNET.sys

- SRV.sys

- SRV2.sys

- SMBdirect.sys

- 低於 **% windir% \\ system32**

- srvsvc.dll

![SMB 元件](media/troubleshooting-smb-2.png)

### <a name="update-suggestions"></a>更新建議

針對 SMB 問題進行疑難排解之前，建議您先更新下列元件：

- 檔案伺服器需要檔案儲存體。 如果您的存放裝置有 iSCSI 元件，請更新這些元件。

- 更新網路元件。

- 為獲得更好的效能和穩定性，請更新 Windows Core。

## <a name="reference"></a>參考

[Microsoft SMB 通訊協定封包交換案例](/windows/win32/fileio/microsoft-smb-protocol-packet-exchange-scenario)
