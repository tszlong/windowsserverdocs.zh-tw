---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: 設定電腦進行疑難排解
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d9d279615dc1f70ffdcff9e49a4aa619f0106a93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822971"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>設定電腦進行疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用先進的疑難排解技術識別並修正 Active Directory 問題之前，請先設定您的電腦以進行疑難排解。 您也應該對疑難排解概念、程式和工具有基本瞭解。

如需 Windows Server 監視工具的相關資訊，請參閱[Windows server 中的效能與可靠性監視](https://go.microsoft.com/fwlink/?LinkId=123737)逐步指南

## <a name="configuration-tasks-for-troubleshooting"></a>疑難排解的設定工作

若要設定電腦以進行疑難排解 Active Directory Domain Services （AD DS），請執行下列工作：

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>安裝 AD DS 的遠端伺服器管理工具

當您安裝 AD DS 來建立網域控制站時，系統會自動安裝您用來管理 AD DS 的系統管理工具。 如果您想要從不是網域控制站的電腦遠端系統管理網域控制站，您可以在執行支援的 Windows 版本的成員伺服器或工作站上安裝遠端伺服器管理工具（RSAT）。 RSAT 取代 Windows Server 2003 的 Windows 支援工具。

如需安裝 RSAT 的詳細資訊，請參閱[遠端伺服器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)一文。

### <a name="configure-reliability-and-performance-monitor"></a>設定可靠性和效能監視器

Windows Server 包含 Windows 可靠性和效能監視器，這是一個 Microsoft Management Console （MMC）嵌入式管理單元，結合了舊版獨立工具的功能，包括效能記錄檔及警示、Server Performance Advisor 和系統監視器。 此嵌入式管理單元提供圖形化使用者介面（GUI），可用於自訂資料收集器集合和事件追蹤會話。

可靠性和效能監視器也包括可靠性監視器，此 MMC 嵌入式管理單元會追蹤系統的變更，並將其與系統穩定性的變更進行比較，以提供其關聯性的圖形化視圖。

### <a name="set-logging-levels"></a>設定記錄層級

如果您在事件檢視器的目錄服務記錄中收到的資訊不足以進行疑難排解，請在**HKEY_LOCAL_MACHINE \system\currentcontrolset\services\ntds\diagnostics**中使用適當的登錄專案來提高記錄層級。

根據預設，所有專案的記錄層級都會設定為**0**，以提供最少的資訊數量。 最高的記錄層級為**5**。 增加專案的層級會導致目錄服務事件記錄檔中的其他事件記錄下來。

請使用下列程式來變更診斷專案的記錄層級。 至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

> [!WARNING]
> 建議您不要直接編輯登錄，除非已沒有其他替代方案。 對登錄所做的修改會不經登錄編輯程式或 Windows 驗證就加以套用，因此可能會儲存不正確的值。 這可能會導致系統發生嚴重錯誤。 可能的話，請使用群組原則或其他 Windows 工具（例如 MMC 嵌入式管理單元）來完成工作，而不是直接編輯登錄。 如果您必須編輯登錄，必須非常小心。
>

變更診斷專案的記錄層級

1. 按一下 [**啟動**] > **執行**> 輸入**Regedit** ，> 按一下 **[確定]** 。
2. 流覽至您要設定登入的專案。
   * 範例： HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. 按兩下專案，然後在 [**基底**] 中按一下 [ **Decimal**]。
4. 在 [**值**] 中，輸入從**0**到**5**的整數，然後按一下 **[確定]** 。
