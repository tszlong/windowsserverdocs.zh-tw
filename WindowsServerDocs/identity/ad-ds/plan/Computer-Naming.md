---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: 電腦命名
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 59b6be118a93881d5800e2f0032e0738c7e3bdfa
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941188"
---
# <a name="computer-naming"></a>電腦命名

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當執行 Windows 2000、Windows XP、Windows Server 2003、Windows Server 2008 或 Windows Vista 作業系統的電腦加入網域時，電腦依預設會將名稱指派給自己。 它本身指派的名稱是由電腦的主機名稱所組成 (也就是 [系統內容] 中的 [電腦名稱稱]) ，而網域名稱系統 (電腦所加入之 Active Directory 網域的 DNS) 名稱 (也就是 [系統屬性]) 中的 [主要 DNS 尾碼]。 主機名稱和網域 DNS 名稱的串連稱為 (FQDN) 的完整功能變數名稱。 例如，如果主機名稱為 Server1 的電腦加入網域 corp.contoso.com，則會 server1.corp.contoso.com 電腦的 FQDN。

如果電腦已經有以靜態方式輸入至 DNS 區域的不同 DNS 功能變數名稱，或是由整合式 DNS/動態主機設定通訊協定所註冊 (DHCP) Server 服務，則電腦的 FQDN 與先前註冊的名稱不同。 您可以使用任何一個名稱來參考電腦。

如需 Active Directory Domain Services (AD DS) 中命名慣例的詳細資訊，請參閱 [Active Directory 中的電腦、網域、網站和 ou 的命名慣例](https://support.microsoft.com/help/909264/)。
