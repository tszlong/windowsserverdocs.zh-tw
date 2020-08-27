---
ms.assetid: fd3bc84a-48eb-4f00-9dc2-846bf2c2668b
title: 對 AD DS 進行疑難排解
description: AD DS 的疑難排解區段總覽
ms.author: iainfou
author: iainfoulds
manager: dcscontentpm
ms.date: 11/22/2019
ms.topic: article
ms.openlocfilehash: 485b78bcefbdf7d399f5bae041c653f300bc817a
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939958"
---
# <a name="ad-ds-troubleshooting"></a>對 AD DS 進行疑難排解

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

本節包含診斷和修正 Active Directory 複寫期間可能發生之問題的疑難排解建議和程式。 它著重于如何回應目錄服務事件記錄檔專案，以及如何解讀 Repadmin.exe 和 Dcdiag.exe 等工具可能會報告的訊息。

所有執行 Windows Server 2012 R2 或更新版本的網域控制站都可使用 Repadmin.exe 和 Dcdiag.exe。 如需如何使用這些工具來疑難排解問題的詳細資訊，請參閱下列文章。

- [設定電腦以進行疑難排解 Active Directory](../manage/troubleshoot/Configuring-a-Computer-for-Troubleshooting.md)
- [進行 Active Directory 複寫問題疑難排解](../manage/troubleshoot/Troubleshooting-Active-Directory-Replication-Problems.md)

另一項實用的技術是 Windows (ETW) 的事件追蹤。 您可以使用 ETW 來疑難排解網域控制站之間的 LDAP 通訊。 如需詳細資訊，請參閱 [使用 ETW 疑難排解 LDAP 連接](../manage/troubleshoot/troubleshoot-ldap-using-etw.md)。

您也可以在執行 Windows 10 的成員伺服器上安裝遠端伺服器管理工具 (RSAT) 。 如需有關如何安裝 RSAT 的詳細資訊，請參閱 [遠端伺服器管理工具](../../../remote/remote-server-administration-tools.md)。
