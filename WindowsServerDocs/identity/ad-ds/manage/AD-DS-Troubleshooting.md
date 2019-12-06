---
ms.assetid: fd3bc84a-48eb-4f00-9dc2-846bf2c2668b
title: 對 AD DS 進行疑難排解
description: AD DS 的疑難排解區段總覽
ms.author: joflore
author: MicrosoftGuyJFlo
manager: dcscontentpm
ms.date: 11/22/2019
ms.topic: article
ms.prod: windows-server
audience: Admin
ms.custom:
- CSSTroubleshoot
ms.technology: identity-adds
ms.openlocfilehash: 500ffe05fe75db99f98fda09b5f86b8547ea7e32
ms.sourcegitcommit: 30de12eebeb0fc79567d6bb6ab513692ea2415d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2019
ms.locfileid: "74853583"
---
# <a name="ad-ds-troubleshooting"></a>對 AD DS 進行疑難排解

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

本節包含診斷和修正 Active Directory 複寫期間可能發生之問題的疑難排解建議和程式。 它著重于如何回應目錄服務事件記錄檔專案，以及如何解讀如 Repadmin 和 Dcdiag.exe 等工具可能會報告的訊息。

在執行 Windows Server 2012 R2 或更新版本的所有網域控制站上都可使用 Repadmin 和 Dcdiag.exe。 如需有關如何使用這些工具來疑難排解問題的詳細資訊，請參閱下列文章。

- [設定電腦進行疑難排解 Active Directory](../manage/troubleshoot/Configuring-a-Computer-for-Troubleshooting.md)
- [進行 Active Directory 複寫問題疑難排解](../manage/troubleshoot/Troubleshooting-Active-Directory-Replication-Problems.md)

另一個有用的技術是 Windows 事件追蹤（ETW）。 您可以使用 ETW 來疑難排解網域控制站之間的 LDAP 通訊。 如需詳細資訊，請參閱[使用 ETW 針對 LDAP 連接進行疑難排解](../manage/troubleshoot/troubleshoot-ldap-using-etw.md)。

您也可以在執行 Windows 10 的成員伺服器上安裝遠端伺服器管理工具（RSAT）。 如需有關如何安裝 RSAT 的詳細資訊，請參閱[遠端伺服器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。
