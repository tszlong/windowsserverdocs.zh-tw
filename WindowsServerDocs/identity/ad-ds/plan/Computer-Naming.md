---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: 電腦命名
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 37f877b3165f5de31c8a26ae4000b8064362fa17
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947846"
---
# <a name="computer-naming"></a>電腦命名

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當執行 Windows 2000、Windows XP、Windows Server 2003、Windows Server 2008 或 Windows Vista 作業系統的電腦加入網域時，根據預設，電腦會指派自己的名稱。 它本身指派的名稱包含電腦的主機名稱 (也就是系統內容中的電腦名稱稱) 和網域名稱系統 (電腦加入之 Active Directory 網域的 DNS) 名稱 (也就是系統屬性) 中的主要 DNS 尾碼。 主機名稱和網域 DNS 名稱的串連，稱為 (FQDN) 的完整功能變數名稱。 例如，如果主機名稱為 Server1 的電腦加入網域 corp.contoso.com，則電腦的 FQDN 會是 server1.corp.contoso.com。

如果電腦的 DNS 功能變數名稱已以靜態方式輸入至 DNS 區域，或由整合式 DNS/動態主機設定通訊協定（ (DHCP) Server 服務）註冊，則電腦的 FQDN 會與先前註冊的名稱不同。 您可以使用任何名稱來參照電腦。

如需 Active Directory Domain Services (AD DS) 中命名慣例的詳細資訊，請參閱[Active Directory 中的電腦、網域、網站和 ou 的命名慣例](https://support.microsoft.com/help/909264/)。
