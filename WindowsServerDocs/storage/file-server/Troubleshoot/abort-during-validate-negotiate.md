---
title: TCP 連線在驗證交涉期間中止
description: 介紹如何在驗證協商期間中止 TCP 連線時，針對 SMB 問題進行疑難排解。
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 762ad3b20c389771a9c0e6783d2a6fb1e73b8f0d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961000"
---
# <a name="tcp-connection-is-aborted-during-validate-negotiate"></a>TCP 連線在驗證交涉期間中止

在 SMB 問題的網路追蹤中，您會注意到在驗證協商過程中發生 TCP 重設中止。 本文說明如何針對這種情況進行疑難排解。

## <a name="cause"></a>原因

此問題可能是因為協商驗證失敗所造成。 這通常是因為 WAN 加速器修改了原始的 SMB NEGOTIATE 封包。

因為任何原因，Microsoft 不再允許修改驗證協商封包。 這是因為此行為會造成嚴重的安全性風險。

下列需求適用于驗證 Negotiate 封包：

- 「驗證協商」程式會使用 FSCTL \_ Validate \_ negotiate \_ INFO 命令。

- 驗證協商回應必須經過簽署。 否則，就會中止連接。

- 您應該將 FSCTL \_ VALIDATE \_ negotiate INFO 訊息與 NEGOTIATE 訊息進行比較， \_ 以確保沒有任何變更。

## <a name="workaround"></a>因應措施

您可以暫時停用驗證協商進程。 若要這麼做，請找出下列登錄子機碼：

**HKEY \_ 本機 \_ 電腦 \\ 系統 \\ CurrentControlSet \\ 服務 \\ LanmanWorkstation \\ 參數**

在**Parameters**索引鍵底下，將**RequireSecureNegotiate**設定為**0**。

在 Windows PowerShell 中，您可以執行下列命令來設定此值：

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecureNegotiate -Value 0 -Force
```

> [!NOTE]
> 無法在 Windows 10、Windows Server 2016 或更新版本的 Windows 中停用驗證協商進程。

如果用戶端或伺服器無法支援 [驗證 Negotiate] 命令，您可以將 SMB 簽署設定為 [必要] 來解決此問題。 SMB 簽署會被視為比驗證 Negotiate 更安全。 不過，如果需要簽署，也可能會降低效能。

## <a name="reference"></a>參考

如需詳細資訊，請參閱下列文章：

[3.3.5.15.12 處理驗證 Negotiate 資訊要求](/openspecs/windows_protocols/ms-smb2/0b7803eb-d561-48a4-8654-327803f59ec6)

[3.2.5.14.12 處理驗證 Negotiate 資訊回應](/openspecs/windows_protocols/ms-smb2/6a5bc90d-3c08-4498-905b-e7dab30b2e0e)
