---
title: 在連線時使用者收到「遠端桌面服務目前忙碌中」訊息
description: 針對使用者啟動遠端桌面連線時，出現遠端桌面服務目前忙碌中的錯誤進行疑難排解。
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: c345833ee63a1286a5615998649e8aa9d25896a6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857161"
---
# <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>在連線時使用者收到「遠端桌面服務目前忙碌中」訊息

若要判斷此問題的適當回應，請查看下列內容：

- 「遠端桌面服務」服務是否沒有回應 (例如，遠端桌面用戶端似乎在歡迎畫面「停止回應」)。  
   - 如果服務變得沒有回應，請參閱 [RDSH 伺服器記憶體問題](#rdsh-server-memory-issue)。
   - 如果用戶端似乎與服務正常互動，請繼續下一個步驟。
- 如果一或多個使用者中斷連線其遠端桌面工作階段，使用者可以重新連線嗎？  
   - 如果不論有多少使用者中斷工作階段連線，服務仍持續拒絕連線，請參閱 [RD 接聽程式問題](#rd-listener-issue)。
   - 如果在一些使用者中斷工作階段連線之後，服務開始再次接受連線，請[檢查連線限制原則](#check-the-connection-limit-policy)。

## <a name="rdsh-server-memory-issue"></a>RDSH 伺服器記憶體問題

在部分 Windows Server 2012 R2 RDSH 伺服器上發現記憶體流失。 經過一段時間，這些伺服器開始拒絕遠端桌面連線和本機主控台登入，並出現如下訊息：

> 無法完成您嘗試執行的工作，因為遠端桌面服務目前忙碌中。 請等候一段時間後再試一次。 其他使用者應仍能登入。

嘗試連線的遠端桌面用戶端也變得沒有回應。

若要解決此問題，請重新啟動 RDSH 伺服器。

若要解決此問題，請將 KB 4093114：[2018 年 4 月10日—KB4093114 (每月匯總套件)](https://support.microsoft.com/help/4093114/) 套用至 RDSH 伺服器。

## <a name="rd-listener-issue"></a>RD 接聽程式問題

在直接從 Windows Server 2008 R2 升級到 Windows Server 2012 R2 或 Windows Server 2016 的某些 RDSH 伺服器上發現問題。 當遠端桌面用戶端連線到 RDSH 伺服器時，RDSH 伺服器會為使用者工作階段建立 RD 接聽程式。 受影響的伺服器會計算 RD 接聽程式的數量，其隨著使用者連線而增加，但永遠不會減少。

您可以使用下列方法解決此問題：

  - 重新啟動 RDSH 伺服器以重設 RD 接聽程式計數
  - 修改連線限制原則，將其設定為非常大的值。 如需管理連線限制原則的詳細資訊，請參閱[檢查連線限制原則](#check-the-connection-limit-policy)。

若要解決此問題，請將下列更新套用到 RDSH 伺服器：

  - Windows Server 2012 R2：KB 4343891，[2018 年 8 月 30 日—KB4343891 (每月彙總套件預覽)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016：KB 4343884，[2018 年 8 月 30 日—KB4343884 (作業系統組建 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)

## <a name="check-the-connection-limit-policy"></a>檢查連線限制原則

您可以在個別電腦層級設定同時遠端桌面連線的數量限制，或藉由設定群組原則物件 (GPO)。 預設未設定限制。

若要檢查目前的設定，並找出 RDSH 伺服器上任何現有的 GPO，請以系統管理員身分開啟命令提示字元視窗，並輸入下列命令：
  
```cmd
gpresult /H c:\gpresult.html
```
   
此命令完成後，開啟 **gpresult.html**。 在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**中，尋找**連線限制數目**原則。

  - 如果此原則的設定為**已停用**，則群組原則不會限制 RDP 連線。
  - 如果此原則的設定為**已啟用**，則請檢查**優勢 GPO**。 如果您需要移除或變更連線限制，請編輯此 GPO。

若要強制執行原則變更，請在受影響的電腦上開啟命令提示字元視窗，並輸入下列命令：
  
```cmd
gpupdate /force
```
  
