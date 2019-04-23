---
title: 簡介語彙基元繫結
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884229"
---
# <a name="introducing-token-binding"></a>簡介語彙基元繫結

>適用於：Windows Server 2016 和 Windows 10

語彙基元繫結通訊協定可讓應用程式和服務以密碼編譯方式將其安全性語彙基元繫結至 TLS 層，來減輕權杖遭竊並重新執行攻擊。 長時間執行、 可唯一識別 TLS [RFC5246] 繫結可以跨越多個 TLS 工作階段和連接。

版本支援：

- Windows 10 版本 1507 – 預設為關閉
    - 語彙基元繫結的通訊協定加入[[草稿-ietf-tokbind-通訊協定-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet 和 HTTP。語彙基元繫結，透過 HTTP SYS 支援[[草稿-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10，1511 版和 1607年和 Windows Server 2016 – 在預設情況下
    - 語彙基元繫結更新的通訊協定[[草稿-ietf-tokbind-通訊協定-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - 新增的語彙基元繫結交涉的 TLS 延伸模組[[草稿-popov-tokbind-交涉-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet 和 HTTP。語彙基元繫結，透過更新 HTTP SYS 支援[[草稿-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 版本 1507年與服務更新[KB4034668](https://support.microsoft.com/kb/KB4034668)，Windows 10 1511年版服務更新[KB4034660](https://support.microsoft.com/kb/KB4034660)，Windows 10 1607年版，Windows Server 2016 服務更新[KB4034658](https://support.microsoft.com/kb/KB4034658)預設語彙基元繫結的通訊協定版本 0.10 – 支援上
    - 語彙基元繫結更新的通訊協定[[草稿-ietf-tokbind-通訊協定-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 新增的語彙基元繫結交涉的 TLS 延伸模組[[草稿-ietf-tokbind-交涉-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 和 HTTP。語彙基元繫結，透過更新 HTTP SYS 支援[[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10 1703年版支援語彙基元繫結的通訊協定版本 0.10 – 預設開啟
    - 語彙基元繫結更新的通訊協定[[草稿-ietf-tokbind-通訊協定-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 新增的語彙基元繫結交涉的 TLS 延伸模組[[草稿-ietf-tokbind-交涉-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 和 HTTP。語彙基元繫結，透過更新 HTTP SYS 支援[[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - 啟用虛擬化型安全性的 Windows 裝置會在受保護的環境執行的作業系統從隔離中保留的語彙基元繫結索引鍵

ASP.NET 支援的資訊位於[.NET Framework 參考來源](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170)。 

如需.NET Framework 的資訊，請參閱下列主題：

- [網路增強功能](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding 類別](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
