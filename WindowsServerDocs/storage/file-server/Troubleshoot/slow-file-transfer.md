---
title: 緩慢的 SMB 檔案傳送速率
description: 介紹如何針對 SMB 檔案傳輸效能問題進行疑難排解。
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: cb575015ea8a8fc6cbc35358103774a931b0b749
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958860"
---
# <a name="slow-smb-files-transfer-speed"></a>緩慢的 SMB 檔案傳送速率

本文提供透過 SMB 進行慢速檔案傳輸速度的建議疑難排解程式。

## <a name="large-file-transfer-is-slow"></a>大型檔案傳輸速度緩慢

如果您發現大型檔案的傳送速率緩慢，請考慮下列步驟：

- 針對未緩衝的 IO （**xcopy/j**或**robocopy/j**）嘗試檔案複製命令。

- 測試儲存速度。 這是因為檔案複製速度會受到儲存速度的限制。

- 檔案複製有時會快速啟動，然後變慢。 請遵循下列指導方針來驗證這種情況：
    
  - 這通常發生于快取或緩衝處理初始複本（在記憶體中或在 RAID 控制器的記憶體快取中），且快取執行時。這會強制將資料直接寫入磁片（寫入）。 這是較慢的進程。
    
  - 使用存放裝置效能監控計數器來判斷儲存體效能是否會隨著時間而降低。 如需詳細資訊，請參閱[SMB 檔案伺服器的效能微調](../../../administration/performance-tuning/role/file-server/smb-file-server.md)。

- 使用 RAMMap （SysInternals）來判斷記憶體中的「對應檔案」使用量是否因為可用記憶體耗盡而增加。

- 尋找追蹤中的封包遺失。 這可能會導致 TCP 擁塞提供者進行節流。

- 針對 SMBv3 和更新版本，請確定 SMB 多重通道已啟用且正常運作。

- 在 SMB 用戶端上，啟用 SMB 中的大型 MTU，並停用頻寬節流設定。 若要這樣做，請執行下列命令：  
  
  ```PowerShell
  Set-SmbClientConfiguration -EnableBandwidthThrottling 0 -EnableLargeMtu 1
  ```

## <a name="small-file-transfer-is-slow"></a>小型檔案傳輸速度緩慢

透過 SMB 進行小型檔案的緩慢傳輸，最常見的原因是有許多檔案。 這是預期中的行為。

檔案傳輸期間，檔案建立會導致高通訊協定額外負荷和高檔案系統額外負荷。 對於大型檔案傳輸，這些成本只會發生一次。 當傳送大量小型檔案時，成本會重複，並導致傳送速率變慢。

以下是有關此問題的技術詳細資料：

- SMB 會呼叫 create 命令來要求建立檔案。 有些程式碼會檢查檔案是否存在，然後建立檔案。 或者，create 命令的某些變化會建立實際的檔案。

- 每個 create 命令都會在檔案系統上產生活動。

- 寫入資料之後，檔案就會關閉。

- 這段期間，程式會受到網路延遲和 SMB 伺服器延遲的影響。 這是因為 SMB 要求會先轉譯成檔案系統命令，然後再轉換成實際的檔案系統延遲，以完成作業。

- 如果有任何防毒程式正在執行，則傳送速率會變慢。 這是因為資料通常會由封包嗅探器一次掃描，而第二次是寫入磁片時。 在某些情況下，這些動作會重複數千次。 您可能會發現小於 1 MB/s 的速度。

## <a name="opening-office-documents-is-slow"></a>開啟 Office 檔的速度很慢

此問題通常發生在 WAN 連線上。 這種情況很常見，通常是由 Office 應用程式（特別是 Microsoft Excel）存取和讀取資料的方式所造成。

我們建議您確定 Office 和 SMB 二進位檔都是最新的，然後透過在 SMB 伺服器上停用租用來進行測試。 若要這樣做，請遵循下列步驟：
   
1. 在 Windows 8 和 Windows Server 2012 或更新版本的 Windows 中執行下列 PowerShell 命令：
      
   ```PowerShell
   Set-SmbServerConfiguration -EnableLeasing $false  
   ```
      
   或者，在提高許可權的 [命令提示字元] 視窗中執行下列命令：  

   ```cmd
   REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters /v DisableLeasing /t REG\_DWORD /d 1 /f  
   ```
      
   > [!NOTE]
   > 設定此登錄機碼之後，將不再授與 SMB2 租用，但仍可使用 oplocks。 此設定主要用於疑難排解。
    
2. 重新開機檔案伺服器，或重新開機**伺服器**服務。 若要重新開機服務，請執行下列命令：

   ```cmd  
   NET STOP SERVER 
   NET START SERVER
   ```

若要避免這個問題，您也可以將檔案複寫到本機檔案伺服器。 如需詳細資訊，請參閱[使用 EFS 時將 Office 檔 aving 到網路伺服器的速度很慢](/office/troubleshoot/office/saving-file-to-network-server-slow)。
