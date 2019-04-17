---
title: "簡介權杖繫結"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="introducing-token-binding"></a>簡介權杖繫結

>適用於：Windows Server 2016 和 Windows 10

繫結權杖通訊協定可讓應用程式與服務密碼編譯將其的安全性權杖繫結至 TLS 層減少權杖竊取與重新執行攻擊。 久、 唯一辨識 TLS [RFC5246] 繫結可以跨多個 TLS 工作階段和連接。

版本支援：

- Windows 10 版本 1507 – 預設為關閉
    - 權杖繫結通訊協定新增[[草稿-ietf-tokbind-通訊協定-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet 與 HTTP。透過 HTTP 權杖繫結 SYS 支援[[草稿-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10，版本 1511年和 1607，與 Windows Server 2016-上預設
    - 權杖繫結通訊協定更新[[草稿-ietf-tokbind-通訊協定-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - 加入權杖繫結交涉 TLS 副檔名[[草稿-popov-tokbind-交涉-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet 與 HTTP。透過 HTTP 更新權杖繫結 SYS 支援[[草稿-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 版本 1507 年的維護更新[KB4034668](https://support.microsoft.com/kb/KB4034668)，Windows 10，版本 1511 年的維護更新[KB4034660](https://support.microsoft.com/kb/KB4034660)，Windows 10，版本 1607 年和 Windows Server 2016 的維護更新[KB4034658](https://support.microsoft.com/kb/KB4034658)上預設支援權杖繫結通訊協定版本 0.10 –
    - 權杖繫結通訊協定更新[[草稿-ietf-tokbind-通訊協定-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 加入權杖繫結交涉 TLS 副檔名[[草稿-ietf-tokbind-交涉-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 與 HTTP。透過 HTTP 更新權杖繫結 SYS 支援[[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10，版本 1703年支援權杖繫結通訊協定版本 0.10 – 在預設
    - 權杖繫結通訊協定更新[[草稿-ietf-tokbind-通訊協定-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 加入權杖繫結交涉 TLS 副檔名[[草稿-ietf-tokbind-交涉-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 與 HTTP。透過 HTTP 更新權杖繫結 SYS 支援[[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - 模擬為基礎的安全性功能的 Windows 裝置將會在執行作業系統隔離的受保護的環境中保留權杖繫結按鍵

可以找到佇列支援的相關資訊， [.NET Framework 參考資料來源](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170)。 

.NET Framework 有關，請查看下列主題：

- [網路美化效果](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding 課](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
