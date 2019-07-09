---
title: 從 Windows Server (1709 版) 開始移除或計劃取代的功能
description: 已移除或已計劃在發行版本中移除的特性與功能。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/05/2018
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 37970f3bee2070cffc77bff855a8f28641196b24
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66452865"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1709"></a>從 Windows Server 1709 版開始移除或計劃取代的功能

>適用於：Windows Server 1709 版

以下清單列出的 Windows Server 1709 版特性與功能已從該版本的產品中移除，或開始考慮可能在後續版本中取代。 適用對象是在商業環境中更新作業系統的 IT 專業人員。 **此清單的後續版本可能會變更，並非所有受影響的特色或功能都包含在其中。** 

## <a name="features-removed-from-windows-server-version-1709"></a>Windows Server 1709 版已移除的功能
Windows Server 1709 版包含 Windows Server 2016 中提供的相同功能。 不過，這個版本提供不同於 Windows Server 2016 的安裝選項：

- 作為半年通道發行版本，Windows Server 1709 版僅提供 Server Core 安裝選項。 如需詳細資訊，請參閱[維護通道比較](../get-started-19/servicing-channels-19.md)。
- 從這個版本開始，Nano Server 無法作為可安裝的主機作業系統。 反之，Nano Server 改以容器作業系統形式來提供。 請參閱 [Nano Server 在 Windows Server 1709 版中的變更](nano-in-semi-annual-channel.md)。
- 從此版本開始，預設不會再安裝伺服器訊息區 (SMB) 1 版。 如需詳細資訊，請參閱 [Windows 10 Fall Creators Update 和 Windows Server 1709 版本及之後的版本皆未預設安裝 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows)。


## <a name="features-being-considered-for-replacement-starting-with-subsequent-releases"></a>考慮要從後續版本開始取代的功能

正在考慮要從 Windows Server 1709 版之後的發行開始取代下列特色及功能。 最後可能都會從已安裝的產品映像完全移除，並由取代其他特色或功能取代 (或是可從其他來源安裝)，這些功能有時會有特定功能遭移除，但在此版本中仍然可用。 您現在就應該開始針對所有依存於這些功能的應用程式、程式碼或使用方式，規劃替代方法或日後取代。

如果您對任何這些功能的建議取代有意見反應要分享，可以使用[意見反應中樞應用程式](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 即使此應用程式執行於 Windows 10 上，您同樣可以用來傳送有關 Windows Server 產品 (和文件) 的意見反應給我們。

### <a name="iis-6-management-compatibility"></a>IIS 6 管理相容性
考慮要取代的特定功能︰

- IIS 6 Metabase 相容性 (Web-Metabase)
- IIS 6 管理主控台 (Web-Lgcy-Mgmt-Console)
- IIS 6 指令碼工具 (Web-Lgcy-Scripting)
- IIS 6 WMI 相容性 (Web-WMI)

不考慮 IIS 6 Metabase 相容性 (當做 IIS 6 Metabase 指令碼與 IIS 7 或更新版本所用檔案型設定之間的模擬層)，而是應該使用 Microsoft.Web.Administration 命名空間等工具，開始移轉管理指令碼，使它們直接以 IIS 檔案型設定為目標。

您也應該開始從 IIS 6.0 或較舊版本進行移轉，並移到一律會在最新發行的 Windows Server 版本中提供的最新版 IIS。


### <a name="iis-digest-authentication"></a>IIS 摘要式驗證
此驗證方法已計劃要進行取代。 您應該開始改用其他驗證方法，例如，用戶端憑證對應 (請參閱[設定一對一用戶端憑證對應](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings) \(英文\)) 或 Windows 驗證 (請參閱[應用程式設定](https://docs.microsoft.com/iis-administration/configuration/appsettings.json) \(英文\))。

### <a name="internet-storage-name-service-isns"></a>網際網路儲存名稱服務 (iSNS)
iSNS 即將列入取代考量中。 伺服器訊息區 (SMB) 功能提供基本上相同的功能，而且還有額外的功能。 如需此功能的背景資訊，請參閱[伺服器訊息區概觀](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx) \(機器翻譯\)。

### <a name="rsaaes-encryption-for-iis"></a>IIS 的 RSA/AES 加密 
正在考慮取代這個加密方法，因為已經有更好的「密碼編譯 API：新一代 (CNG)」方法可以使用。 若要深入了解 CNG 加密，請參閱[關於 CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx) \(英文\)。

### <a name="windows-powershell-20"></a>Windows PowerShell 2.0
這個早期 Windows PowerShell 版本已被數個較新的版本取代。 為獲得最佳功能及效能，請遷移至 Windows PowerShell 5.0 或更新版本。 如需詳細資訊，請參閱 [PowerShell 文件](https://docs.microsoft.com/powershell/index?view=powershell-5.1)。

