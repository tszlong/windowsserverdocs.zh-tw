---
title: DirectAccess 已知問題
description: 本主題提供 Windows Server 2016 中 DirectAccess 的 Microsoft 技術支援檔連結。
manager: brianlic
ms.topic: article
ms.assetid: 3511a91f-1d5d-45a0-97f2-3fc0d6f079b4
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 117e598542ff0140c1cacc533e26d85174f0fcc6
ms.sourcegitcommit: eb995fa887ffe1408b9f67caf743c66107173666
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2021
ms.locfileid: "98666567"
---
# <a name="directaccess-known-issues"></a>DirectAccess 已知問題

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2019

## <a name="dns-registration-of-directaccess-client-ipv6-addresses"></a>DirectAccess 用戶端 IPv6 位址的 DNS 註冊

從 Windows 10 5 月2020更新開始，用戶端就不會再于名稱解析原則表中設定的 DNS 伺服器上登錄其 IP 位址 (NRPT) 。
如果需要 DNS 註冊（例如， **管理**），可以在用戶端上使用這個登錄機碼來明確啟用它：

路徑：`HKLM\System\CurrentControlSet\Services\Dnscache\Parameters`<br/>
輸入： `DWORD`<br/>
值名稱：`DisableNRPTForAdapterRegistration`<br/>
值：<br/>
`1` -DNS 註冊已停用 (預設，因為 Windows 10 可能是2020更新) <br/>
`0` -已啟用 DNS 註冊

## <a name="recommended-hotfixes-and-updates-for-windows-server-2012-directaccess"></a>Windows Server 2012 DirectAccess 的建議修補和更新
下列連結會列出適用于 DirectAccess 的 Microsoft 技術支援檔，您應該在開始部署之前加以審查並套用，以避免無法使用的設定。

-   [Windows Server 2012 DirectAccess 的建議修補和更新](https://support.microsoft.com/kb/2883952)


