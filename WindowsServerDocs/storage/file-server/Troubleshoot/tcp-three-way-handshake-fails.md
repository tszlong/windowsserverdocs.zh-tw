---
title: SMB 連線期間的 TCP 三向交握失敗
description: 引進 SMB 連線期間的 TCP 三向交握失敗。
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 8cef47e164b8768747cb383f4d7012130c7cb516
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654629"
---
# <a name="tcp-three-way-handshake-failure-during-smb-connection"></a>SMB 連線期間的 TCP 三向交握失敗

當您分析網路追蹤時，您會發現發生 SMB 問題的傳輸控制通訊協定（TCP）三向交握失敗。 本文說明如何針對這種情況進行疑難排解。

## <a name="troubleshooting"></a>[疑難排解]

一般來說，原因是封鎖流量的本機或基礎結構防火牆。 此問題可能會發生在下列任一情況下。

## <a name="scenario-1"></a>案例 1

TCP SYN 封包會抵達 SMB 伺服器，但 SMB 伺服器不會傳回 TCP SYN ACK 封包。

若要針對此案例進行疑難排解，請遵循下列步驟。

### <a name="step-1"></a>步驟 1

執行**netstat**或**NetTcpConnection** ，以確定在 TCP 通訊埠445上有一個接聽程式應由系統進程所擁有。

```cmd
netstat -ano | findstr :445
```

```PowerShell
Get-NetTcpConnection -LocalPort 445
```

### <a name="step-2"></a>步驟 2

確定伺服器服務已啟動且正在執行。

### <a name="step-3"></a>步驟 3

採取 Windows 篩選平台（WFP）捕捉，判斷哪個規則或程式正在卸載流量。 若要這麼做，請在 [命令提示字元] 視窗中執行下列命令：

```cmd
netsh wfp capture start
```

重現問題，然後執行下列命令：

```cmd
netsh wfp capture stop
```

執行案例追蹤，並尋找在 SMB 流量（TCP 通訊埠445）中的 WFP 下降。

（選擇性）您可以移除防毒程式，因為它們不一定是以 WFP 為基礎。

### <a name="step-4"></a>步驟 4

如果已啟用 Windows 防火牆，請啟用防火牆記錄，以判斷它是否會記錄流量的下降。

請確定已在 [**具有 Advanced Security 的 Windows 防火牆**] \>**輸入規則**中，啟用適當的「檔案及印表機共用（SMB）」規則。

> [!NOTE]
> 根據您電腦的設定方式而定，「Windows 防火牆」可能稱為「Windows Defender 防火牆」。

## <a name="scenario-2"></a>案例 2

TCP SYN 封包永遠不會抵達 SMB 伺服器。

在此案例中，您必須沿著網路路徑調查裝置。 您可以分析每個裝置上所捕捉的網路追蹤，以判斷哪個裝置正在封鎖流量。
