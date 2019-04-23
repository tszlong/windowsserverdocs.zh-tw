---
title: 適用於 Windows Server 的 Windows Defender 概觀
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-defender
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 751efb33-a08e-4e90-9208-6f2bc319e029
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 29506acf9ee7c52e100eb278c53205d03ef472d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855619"
---
# <a name="windows-defender-antivirus-for-windows-server"></a>適用於 Windows Server 的 Windows Defender 防毒軟體

>適用於：Windows Server 2016

Windows Server 2016 現在包含 Windows Defender 防毒軟體。 Windows Defender AV 是惡意程式碼保護，可立即且主動地防止已知的惡意程式碼中的 Windows Server 2016，並可以定期更新反惡意程式碼定義，透過 Windows Update。

請參閱[在 Windows 10 的 Windows Defender 防毒軟體](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)文件庫，如需詳細資訊。


雖然 Windows Defender 防毒軟體的功能、設定，以及管理在大部分的情況下無論是在 Windows 10 或 Windows Server 2016 上都相同，但仍有幾個重要的不同：

- 在 Windows Server 2016 中，[自動排除範圍](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)會根據您定義的伺服器角色進行套用。
- 在 Windows Server 2016 中，Windows Defender 防毒軟體並不會在您執行其他防毒軟體產品時自動停用。

[Windows Server 2016 上的 Windows Defender 防毒軟體](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)主題包含設定和 Windows Server 2016 中，特定設定資訊包括如何：

-   [啟用的介面](https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_UsingDef)

-   [確認已在執行 Windows Defender AV]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefRun)

-   [更新反惡意程式碼定義]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_UpdateDef)

-   [提交範例]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefSamples)

-   [設定自動排除項目]( https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016#BKMK_DefExclusions)
