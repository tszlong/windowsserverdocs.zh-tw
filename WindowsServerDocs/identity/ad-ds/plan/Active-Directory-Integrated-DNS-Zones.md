---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Active Directory 整合 DNS 區域
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: be71b719853f82338769d08d608caf8935add672
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962390"
---
# <a name="active-directory-integrated-dns-zones"></a>Active Directory 整合 DNS 區域

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在網域控制站上執行的網域名稱系統（DNS）伺服器可以將其區域儲存在 Active Directory Domain Services （AD DS）中。 如此一來，就不需要設定使用一般 DNS 區域傳輸的個別 DNS 複寫拓撲，因為所有區域資料都會透過 Active Directory 複寫自動複寫。 這會簡化部署 DNS 的程式，並提供下列優點：

- DNS 複寫會建立多個主要複本。 因此，執行 DNS 伺服器服務的網域中的任何網域控制站都可以針對其授權的功能變數名稱，將更新寫入 Active Directory 整合的 DNS 區域。 不需要個別的 DNS 區域傳輸拓撲。

- 支援安全動態更新。 安全動態更新可讓系統管理員控制哪些電腦會更新哪些名稱，以及防止未經授權的電腦覆寫 DNS 中的現有名稱。

Windows Server 2008 中 Active Directory 整合式 DNS 會將區域資料儲存在應用程式目錄分割中。 （與 Active Directory 的 Windows Server 2003 DNS 整合不會有任何行為變更）。在 AD DS 安裝期間，會建立下列 DNS 特定的應用程式目錄分割：

- 整個樹系的應用程式目錄分割，稱為 ForestDnsZones

- 樹系中每個網域的全網域應用程式目錄分割，名為 DomainDnsZones

如需有關 AD DS 如何在應用程式分割區中儲存 DNS 資訊的詳細資訊，請參閱[DNS 技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc779926(v=ws.10))資料。

> [!NOTE]
> 我們建議您在執行 Active Directory Domain Services 安裝精靈（Dcpromo.exe）時安裝 DNS。 如果這樣做，精靈會自動建立 DNS 區域委派。 如需詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。
