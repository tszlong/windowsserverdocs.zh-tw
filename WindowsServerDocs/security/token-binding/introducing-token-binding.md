---
title: 權杖系結簡介
ms.prod: windows-server
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: d067db04fe881193143104ce9f75a0c9932907e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855891"
---
# <a name="introducing-token-binding"></a>權杖系結簡介

>適用于： Windows Server 2016 和 Windows 10

權杖系結通訊協定可讓應用程式和服務以密碼編譯方式將其安全性權杖系結至 TLS 層，以減少權杖遭竊和重新執行攻擊。 長期、唯一可識別的 TLS [RFC5246] 系結可以跨越多個 TLS 會話和連接。

版本支援：

- Windows 10 版本1507–預設為關閉
    - 新增的權杖系結通訊協定[[草稿-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP。透過 HTTP 進行權杖系結的 SYS 支援[[draft-ietf-tokbind-HTTPs-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10，版本1511和1607，以及 Windows Server 2016 –預設為開啟
    - 權杖系結通訊協定已更新[[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - 已新增權杖系結協調的 TLS 延伸[[draft-popov-tokbind-談判-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP。透過 HTTP 更新之權杖系結的 SYS 支援[[draft-ietf-tokbind-HTTPs-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 版本1507（含服務更新[KB4034668](https://support.microsoft.com/kb/KB4034668)、windows 10、版本1511與服務更新[KB4034660](https://support.microsoft.com/kb/KB4034660)、windows 10、版本1607和 Windows Server 2016 with 服務更新[KB4034658](https://support.microsoft.com/kb/KB4034658)支援權杖系結通訊協定版本0.10 –預設為開啟）
    - 權杖系結通訊協定已更新[[draft-ietf-tokbind-通訊協定-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 已新增權杖系結協調的 TLS 延伸[[草稿-ietf-tokbind-----05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP。透過 HTTP 更新對權杖系結的 SYS 支援[[draft-ietf-tokbind-HTTPs-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- 根據預設，Windows 10 版本1703支援權杖系結通訊協定版本0.10 –開啟
    - 權杖系結通訊協定已更新[[draft-ietf-tokbind-通訊協定-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 已新增權杖系結協調的 TLS 延伸[[草稿-ietf-tokbind-----05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP。透過 HTTP 更新對權杖系結的 SYS 支援[[draft-ietf-tokbind-HTTPs-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - 啟用虛擬化安全性的 Windows 裝置會將權杖系結金鑰放在受保護的環境中，並與正在執行的作業系統隔離

如需 ASP .NET 支援的相關資訊，請參閱[.NET Framework 參考來源](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170)。 

如需 .NET Framework 的詳細資訊，請參閱下列主題：

- [網路增強功能](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding 類別](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
