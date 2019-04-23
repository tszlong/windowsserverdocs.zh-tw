---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: 設定電腦進行疑難排解
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1acb5f7d309d58ed4a5a3aca6bb89f01c0cbf933
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854119"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>設定電腦進行疑難排解

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用進階的疑難排解技巧，找出並修正 Active Directory 問題之前，設定您的電腦進行疑難排解。 您也應該了解的基本概念、 程序和工具進行疑難排解。

監視工具，適用於 Windows Server 的相關資訊，請參閱逐步指南 》[效能和可靠性監控 Windows Server 中](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>設定工作，以進行疑難排解

若要設定您的電腦進行疑難排解 Active Directory 網域服務 (AD DS)，請執行下列工作：

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>適用於 AD DS 安裝遠端伺服器管理工具

當您安裝 AD DS 以建立網域控制站時，您用來管理 AD DS 系統管理工具會自動安裝。 如果您想要從不是網域控制站的電腦遠端管理的網域控制站，您可以在成員伺服器或正在執行支援的 Windows 版本的工作站上安裝遠端伺服器管理工具 (RSAT)。 RSAT 會取代從 Windows Server 2003 的 Windows 支援工具。

如需安裝 RSAT 的詳細資訊，請參閱文章[遠端伺服器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

### <a name="configure-reliability-and-performance-monitor"></a>設定 可靠性和效能監視器

Windows Server 包含的 Windows 可靠性和效能監視器 」，它是一個 Microsoft Management Console (MMC) 嵌入式管理單元，結合了先前獨立工具，包括效能記錄及警示，伺服器效能警告器的功能和系統監視器。 此嵌入式管理單元提供圖形化使用者介面 (GUI) 自訂資料收集器集合工具和事件追蹤工作階段。

可靠性和效能監視器也包括可靠性監視器，MMC 嵌入式管理單元，會追蹤系統的變更，然後變更加以比較，在系統穩定性，或提供其關聯性的圖形化檢視。

### <a name="set-logging-levels"></a>設定記錄層級

如果您在事件檢視器中的目錄服務記錄檔中收到的資訊不足，無法進行疑難排解，請使用中的適當的登錄項目引發記錄層級**HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**。

根據預設，若要設定記錄層級的所有項目**0**，以提供資訊的最小數量。 最高的記錄層級**5**。 增加的項目層級會導致其他目錄服務事件記錄檔中記錄的事件。

您可以使用下列程序來變更記錄層級的診斷項目。 若要完成此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

> [!WARNING]
> 建議您不要直接編輯登錄，除非已沒有其他替代方案。 登錄的修改不會驗證登錄編輯程式或 Windows 之前套用，而且如此一來，可以儲存不正確的值。 這可能導致無法復原的錯誤，在系統中。 可能的話，請使用群組原則或其他 Windows 工具，例如 MMC 嵌入式管理單元，來完成工作，而不是直接編輯登錄。 如果您必須編輯登錄，必須非常小心。
>

若要變更記錄層級的診斷項目

1. 按一下 **開始** > **執行**> 類型**regedit** > 按一下 **確定**。
2. 瀏覽至您要設定登入的項目。
   * 範例：HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. 按兩下項目，然後在**基底**，按一下**十進位**。
4. 在 **值**，輸入的整數**0**透過**5**，然後按一下 **確定**。
