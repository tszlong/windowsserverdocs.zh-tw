---
title: HTTP 1.1/2 的效能微調
description: 效能微調建議的 HTTP 1.1/2
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bf85efa88e377966135c23a548119f19c39cceba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866369"
---
# <a name="performance-tuning-http-112"></a>效能微調 HTTP 1.1/2

HTTP/2 是要改善效能，在用戶端 （例如，頁面載入時間在瀏覽器上）。 在伺服器上，則可能代表稍微增加 CPU 成本。 伺服器不再需要為每個要求的單一 TCP 連線，而一些該狀態現在會保持在 HTTP 層。 此外，HTTP/2 有標頭壓縮，它會表示額外的 CPU 負載。

有些情況需要後援 （重設的 HTTP/2 連線，並改為建立新的連接，若要使用 HTTP/1.1） HTTP/1.1。 特別的是，TLS 重新交涉和 HTTP 驗證 （非基本與摘要式） 需要 HTTP/1.1 後援。 即使這增加負擔，這些作業已經代表一些延遲，並因此不會有特別重視效能。

## <a name="see-also"></a>另請參閱
- [Web 伺服器的效能微調](index.md) 
- [IIS 10.0 效能微調](tuning-iis-10.md)