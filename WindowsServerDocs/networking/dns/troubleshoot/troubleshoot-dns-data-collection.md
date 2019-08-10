---
title: 疑難排解網域名稱系統 (DNS) 問題
description: 本文介紹如何在發生 DNS 問題時收集資料。
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: a86b1f34c3b21f5bcde710e2a98323492ea51b62
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917775"
---
# <a name="troubleshooting-domain-name-system-dns-issues"></a>疑難排解網域名稱系統 (DNS) 問題
 
功能變數名稱解析問題可以細分成用戶端和伺服器端的問題。 一般而言, 您應該從用戶端疑難排解開始著手, 除非您在範圍階段判斷問題確實發生在伺服器端。

- [疑難排解 DNS 用戶端](troubleshoot-dns-client.md)

- [疑難排解 DNS 伺服器](troubleshoot-dns-server.md)
 
## <a name="data-collection"></a>資料收集
 
我們建議您在發生問題時, 同時在用戶端和伺服器端同時收集資料。 不過, 視實際問題而定, 您可以在 DNS 用戶端或 DNS 伺服器上的單一資料集啟動集合。
 
若要從受影響的用戶端和其設定的 DNS 伺服器收集 Windows 網路診斷, 請遵循下列步驟:

1. 在用戶端和伺服器上啟動網路捕獲:

   ```cmd
   netsh trace start capture=yes tracefile=c:\%computername%_nettrace.etl
   ```

2. 執行下列命令, 以清除 DNS 用戶端上的 DNS 快取:

   ```cmd
   ipconfig /flushdns
   ```

3. 重現問題。

4. 停止並儲存追蹤:

   ```cmd
   netsh trace stop
   ```

5. 儲存每部電腦的 Nettrace .cab 檔案。 當您聯繫 Microsoft 支援服務時, 這種資訊會很有説明。