---
description: 深入瞭解： Active Directory-Integrated DNS 區域
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Active Directory 整合 DNS 區域
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d07e6ae9751d299cb2f81edb20b036a1cfe63048
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050126"
---
# <a name="active-directory-integrated-dns-zones"></a>Active Directory 整合 DNS 區域

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網域名稱系統 (在網域控制站上執行的 DNS) 伺服器可以將其區域儲存在 Active Directory Domain Services (AD DS) 中。 如此一來，就不需要設定使用一般 DNS 區域傳輸的個別 DNS 複寫拓撲，因為所有區域資料都會透過 Active Directory 複寫來自動複寫。 這可簡化部署 DNS 的程式，並提供下列優點：

- 系統會為 DNS 複寫建立多個主機。 因此，執行 DNS 伺服器服務之網域中的任何網域控制站都可以將更新寫入 Active Directory 整合的 DNS 區域，以取得其所授權之功能變數名稱的更新。 不需要個別的 DNS 區域傳輸拓撲。

- 支援安全動態更新。 安全動態更新可讓系統管理員控制哪些電腦會更新哪些名稱，以及防止未經授權的電腦覆寫 DNS 中的現有名稱。

Windows Server 2008 中 Active Directory 整合式 DNS 會將區域資料儲存在應用程式目錄分割中。  (與 Active Directory 的 Windows Server 2003 型 DNS 整合沒有任何行為變更。 ) 安裝 AD DS 時，會建立下列 DNS 特定的應用程式目錄分割：

- 整個樹系的應用程式目錄分割，稱為 ForestDnsZones

- 樹系中每個網域的全網域應用程式目錄分割，名為 DomainDnsZones

如需 AD DS 如何將 DNS 資訊儲存在應用程式磁碟分割中的詳細資訊，請參閱 [DNS 技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc779926(v=ws.10))。

> [!NOTE]
> 建議您在執行 Active Directory Domain Services 安裝精靈 ( # A0) 時安裝 DNS。 如果這樣做，精靈會自動建立 DNS 區域委派。 如需詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。
