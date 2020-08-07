---
title: HTTP 1.1/2 的效能微調
description: HTTP 1.1/2 的效能微調建議
ms.topic: article
ms.author: ivanpash; gmonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 362ceea398e13c5e537d1d828b86eec8b5d66a8f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896021"
---
# <a name="performance-tuning-http-112"></a>效能微調 HTTP 1.1/2

HTTP/2 是用來改善用戶端上的效能 (例如，瀏覽器) 的頁面載入時間。 在伺服器上，它可能代表 CPU 成本稍微增加。 伺服器不需要針對每個要求使用單一 TCP 連線，而其中一些狀態現在會保留在 HTTP 層中。 此外，HTTP/2 具有標頭壓縮，這代表額外的 CPU 負載。

在某些情況下，需要 HTTP/1.1 回復 (重設 HTTP/2 連線，並改為使用 HTTP/1.1) 建立新的連接。 特別是，除了基本和) 摘要以外，TLS 重新協商和 HTTP 驗證 (需要 HTTP/1.1 回溯。 雖然這會增加額外負荷，但這些作業已經代表一些延遲，因此不會特別區分效能。

## <a name="additional-references"></a>其他參考資料
- [Web 服務器效能調整](index.md)
- [IIS 10.0 效能調整](tuning-iis-10.md)