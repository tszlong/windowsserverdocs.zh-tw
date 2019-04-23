---
ms.assetid: ''
title: Windows 時間服務功能在 Windows Server 2019 insider preview
description: Windows Server 2019 的新 Windows 時間服務功能
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ef0ff317f5957add5ecbe9f88ef83753b805ec41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829019"
---
# <a name="insider-preview"></a>Insider Preview 


## <a name="leap-second-support"></a>閏秒支援


>適用於：Windows Server 2019 和 Windows 10 版本 1809

閏秒是偶爾 1 秒調整為 UTC。 在地球旋轉變慢，因為[UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) （不可部分完成的時幅） 分離[平均陽曆時間](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time)或天文的時間。  一旦 UTC 已發散最.9 秒[閏秒](https://en.wikipedia.org/wiki/Leap_second)插入保留 UTC 與同步 mean 陽曆的時間。

閏秒已成為一定要符合的精確度和在美國和歐洲經濟共同體可追蹤性法規需求。

如需詳細資訊，請參閱：

-  我們[公告部落格](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  驗證指南[開發人員](https://aka.ms/Dev-LeapSecond)

-  驗證指南[IT 專業人員](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>有效位數時間通訊協定

>適用於：Windows Server 2019 和 Windows 10 版本 1809

新的時間提供者，包含在 Windows Server 2019 和 Windows 10 （版本 1809年） 可讓您使用 「 有效位數時間通訊協定 (PTP) 的時間同步。 因為時間分配網路時，遇到的延遲 （延遲），如果不計算，或如果您不對稱，變得越來越困難了解時間伺服器傳來的時間戳記。 PTP 可讓網路裝置，將每個網路裝置所引入的時間測量值，進而提供更精確的時間範例，windows 用戶端的延遲。

如需詳細資訊，請參閱：

-  我們[公告部落格](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  驗證指南[IT 專業人員](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>軟體的時間戳記

>適用於：Windows Server 2019 和 Windows 10 版本 1809

當接收計時封包透過網路從時間伺服器，必須進行處理由作業系統的網路堆疊之前所耗用的時間服務中。 網路堆疊中的每個元件導入了數量不一影響的計時測量精確度的延遲。

![軟體的時間戳記](../media/Windows-Time-Service/software-timestamping.png)

若要解決此問題，軟體時間戳記可讓我們將時間戳記的封包之前和之後 「 Windows 網路功能元件 」 在作業系統中延遲的考量，如上所示。

如需詳細資訊，請參閱：

-  我們[公告部落格](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  驗證指南[IT 專業人員](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---