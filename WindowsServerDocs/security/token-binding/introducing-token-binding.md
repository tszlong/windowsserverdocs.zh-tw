---
description: 深入瞭解：權杖系結簡介
title: Token 系結簡介
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
author: justinha
ms.author: Justinha
ms.date: 11/09/2016
ms.openlocfilehash: 8525ea1a052b4e2c4aaf8e40ed703172964092d3
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044076"
---
# <a name="introducing-token-binding"></a>Token 系結簡介

>適用于： Windows Server 2016 和 Windows 10

權杖系結通訊協定可讓應用程式和服務以密碼編譯方式將其安全性權杖系結至 TLS 層，以降低權杖遭竊和重新執行攻擊。
長期、可唯一識別的 TLS [RFC5246] 系結可以橫跨多個 TLS 會話和連接。

版本支援：

- Windows 10，版本1507–預設為關閉
    - 已新增權杖系結通訊協定 [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP.SYS 透過 HTTP 的權杖系結支援 [[tokbind-HTTPs-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10，版本1511和1607，以及 Windows Server 2016 –預設為開啟
    - 權杖系結通訊協定已更新 [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - 已新增權杖系結協調的 TLS 擴充功能 [[draft-popov-tokbind-協商-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP.SYS 透過 HTTP 更新權杖系結的支援 [[draft-ietf-tokbind-HTTPs-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10，版本1507（含服務更新 [KB4034668](https://support.microsoft.com/kb/KB4034668)、Windows 10、1511版與服務更新 [KB4034660](https://support.microsoft.com/kb/KB4034660)、Windows 10、1607版及 Windows Server 2016 （含服務更新 [KB4034658](https://support.microsoft.com/kb/KB4034658) ）支援權杖系結通訊協定版本0.10 –預設為開啟
    - 權杖系結通訊協定已更新 [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 已新增權杖系結協商的 TLS 延伸 [[draft-ietf-tokbind--05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP.SYS 透過 HTTP 更新權杖系結的支援 [[draft-ietf-tokbind-HTTPs-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10，版本1703預設支援權杖系結通訊協定0.10 版–開啟
    - 權杖系結通訊協定已更新 [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 已新增權杖系結協商的 TLS 延伸 [[draft-ietf-tokbind--05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP.SYS 透過 HTTP 更新權杖系結的支援 [[draft-ietf-tokbind-HTTPs-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - 啟用虛擬化安全性的 Windows 裝置會將權杖系結機碼保留在與執行中作業系統隔離的受保護環境中

您可以在 [.NET Framework 參考來源](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170)中找到 ASP .net 支援的相關資訊。

如需 .NET Framework 的詳細資訊，請參閱下列主題：

- [網路增強功能](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding 類別](/dotnet/api/system.security.authentication.extendedprotection.tokenbinding?view=netframework-4.8)
