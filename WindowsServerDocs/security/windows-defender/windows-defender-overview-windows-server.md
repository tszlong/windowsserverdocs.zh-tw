---
title: Windows Server 的 windows Defender 總覽
description: Windows Server 安全性
ms.topic: article
ms.assetid: 751efb33-a08e-4e90-9208-6f2bc319e029
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: af8ce405409c36f517dd8cc7ab2bc98184eababc
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989779"
---
# <a name="windows-defender-antivirus-for-windows-server"></a>適用于 Windows Server 的 windows Defender 防毒軟體

>適用於：Windows Server 2016

Windows Server 2016 現在包含 Windows Defender 防毒軟體。 Windows Defender AV 是一種惡意程式碼防護，可以立即並主動保護 Windows Server 2016 免于已知的惡意程式碼，並可透過 Windows Update 定期更新反惡意程式碼定義。

如需詳細資訊，請參閱 windows 10 文件庫[中的 Windows Defender 防毒軟體](/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)。


雖然 Windows Defender 防毒軟體的功能、設定，以及管理在大部分的情況下無論是在 Windows 10 或 Windows Server 2016 上都相同，但仍有幾個重要的不同：

- 在 Windows Server 2016 中，[自動排除範圍](/windows/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)會根據您定義的伺服器角色進行套用。
- 在 Windows Server 2016 中，Windows Defender 防毒軟體並不會在您執行其他防毒軟體產品時自動停用。

Windows [server 2016 上的 Windows Defender 防毒軟體](/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)主題包含適用于 windows server 2016 的特定設定資訊，包括如何：

-   [啟用介面](/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_UsingDef)

-   [驗證 Windows Defender 防毒軟體正在執行]( /windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefRun)

-   [更新反惡意程式碼軟體定義]( /windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_UpdateDef)

-   [提交樣本]( /windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefSamples)

-   [設定自動排除]( /windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefExclusions)