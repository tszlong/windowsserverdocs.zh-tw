---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 4e8e3234b89630bf16148eef644f0c6607ad38bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852339"
---
## <a name="importance-of-time-protocols"></a>時間通訊協定的重要性
交換時間資訊，並接著使用該資訊來同步處理其時鐘的兩部電腦之間的通訊時間通訊協定。 與 Windows 時間服務的時間通訊協定，用戶端會要求來自伺服器的時間資訊，並同步處理其時鐘根據收到的資訊。
  
Windows 時間服務會使用 NTP 來協助在網路上同步處理時間。 NTP 是網際網路時間通訊協定，其中包含所需的同步處理時鐘的專業領域演算法。 NTP 是更精確的時間通訊協定比簡單網路時間通訊協定 (SNTP)，可在部分版本的 Windows;不過，W32Time 會繼續支援 SNTP 啟用回溯相容性的執行 SNTP 為基礎的時間服務，例如 Windows 2000 的電腦。

有許多不同的原因，您可能需要精確的時間。  Windows 的一般案例是精確度的 Kerberos，需要 5 分鐘的用戶端與伺服器之間。  不過，有許多可能會受到時間精確度包括其他領域：


- 例如政府法規：
    - 位於美國的 FINRA 的 50 毫秒精確度
    - 1 ms ESMA (MiFID II) 在歐盟境內。
- 密碼編譯演算法
- 叢集/SQL/交換和文件 Db 等分散式的系統
- 比特幣交易的區塊鏈架構
- 分散式記錄檔和威脅分析 
- AD 複寫
- PCI （支付卡產業），目前 1 的第二個精確度