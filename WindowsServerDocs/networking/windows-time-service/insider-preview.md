---
ms.assetid: ''
title: Windows Server 2019 中的 Insider preview for Windows Time 服務功能
description: Windows Server 2019 中新的 Windows Time 服務功能
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 38682afa37a4c6882ee2e63a4abf4cd9fdbd2b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405216"
---
# <a name="insider-preview"></a>Insider Preview 


## <a name="leap-second-support"></a>Leap 第二次支援


>適用於：Windows Server 2019 和 Windows 10，版本1809

第二個是 UTC 偶爾的1秒調整。 隨著地球旋轉的速度變慢， [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) （不可部分完成的時間軸）會從[平均日光時間](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time)或天文時間分歧。  一旦 UTC 在最 .9 的秒內分歧，就會插入[閏秒](https://en.wikipedia.org/wiki/Leap_second)以保持 utc 的同步處理與平均日光時間。

閏秒已經變得很重要，以符合美國和歐盟的正確性和可追蹤性法規需求。

如需詳細資訊，請參閱：

-  我們的[公告 blog](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  [開發人員](https://aka.ms/Dev-LeapSecond)的驗證指南

-  [IT 專業人員](https://aka.ms/ITPro-LeapSecond)的驗證指南


## <a name="precision-time-protocol"></a>精確度時間通訊協定

>適用於：Windows Server 2019 和 Windows 10，版本1809

Windows Server 2019 和 Windows 10 （版本1809）中包含的新時間提供者，可讓您使用精確度時間通訊協定（PTP）來同步處理時間。 當時間分散于網路時，它會遇到延遲（延遲），如果未列入考慮，或如果不是對稱的，就會越來越難以瞭解從時間伺服器傳送的時間戳記。 PTP 讓網路裝置能夠將每個網路裝置引進的延遲新增到計時測量，進而提供更精確的時間取樣給 windows 用戶端。

如需詳細資訊，請參閱：

-  我們的[公告 blog](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  [IT 專業人員](https://aka.ms/PTPValidation)的驗證指南


## <a name="software-timestamping"></a>軟體時間戳記

>適用於：Windows Server 2019 和 Windows 10，版本1809

透過網路從時間伺服器接收計時封包時，必須先由作業系統的網路堆疊處理，然後才能在時間服務中使用。 網路堆疊中的每個元件引進了變動量的延遲，會影響計時測量的精確度。

![軟體時間戳記](../media/Windows-Time-Service/software-timestamping.png)

為了解決這個問題，軟體時間戳記可讓我們在上面所示的「Windows 網路元件」之前和之後加上時間戳記，以考慮作業系統中的延遲。

如需詳細資訊，請參閱：

-  我們的[公告 blog](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  [IT 專業人員](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)的驗證指南



---