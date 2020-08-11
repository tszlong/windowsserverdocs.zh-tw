---
title: Windows 大量啟用疑難排解
description: 列出可提供大量啟用最佳做法相關資訊，以及啟用問題疑難排解相關資訊的資源
ms.topic: troubleshooting
ms.date: 09/24/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: ce6c2e830e7c30e24112854b54e12909c43fa6f7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972325"
---
# <a name="troubleshooting-windows-volume-activation"></a>針對 Windows 大量啟用進行疑難排解

產品啟用是在特定電腦上安裝軟體之後，驗證該軟體的程序。 啟用會確認產品是否為正版軟體 (而非不實複本)，且產品金鑰或序號是有效的，且沒有被盜用或撤銷。 啟用也會建立產品金鑰與安裝之間的連結或關係。

大量啟用是啟用大量授權產品的程序。 若要成為大量授權客戶，組織必須與 Microsoft 簽訂大量授權合約。 Microsoft 會提供自訂的大量授權方案，以配合組織的規模與採購偏好。 如需詳細資訊，請前往 [Microsoft 大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。

[Windows Server 2016 啟用指南](server-2016-activation.md)著重於金鑰管理服務 (KMS) 啟用技術。 本節會處理常見問題，並提供 KMS 和數種其他大量啟用技術的疑難排解指導方針。

## <a name="best-practices-for-volume-activation"></a>大量啟用最佳做法

下列文章提供 Microsoft 大量啟用技術的技術資訊和最佳做法。

### <a name="key-management-service-kms"></a>金鑰管理服務 (KMS)

- [規劃大量啟用](/windows/deployment/volume-activation/plan-for-volume-activation-client)
- [了解 KMS](/previous-versions/tn-archive/ff793434(v=technet.10))
- [部署 KMS 啟用](/previous-versions/tn-archive/ff793409%28v=technet.10%29)
- [設定 KMS 主機](/previous-versions/tn-archive/ff793407%28v%3dtechnet.10%29)
- [設定 DNS](/previous-versions/tn-archive/ff793405%28v%3dtechnet.10%29)
- [使用金鑰管理服務進行啟用](/windows/deployment/volume-activation/activate-using-key-management-service-vamt)

### <a name="active-directory-based-activation-adba"></a>Active Directory 型啟用 (ADBA)

- [部署 Active Directory 型啟用](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502534%28v%3dws.11%29)
- [使用 Active Directory 型啟用進行啟用](/windows/deployment/volume-activation/activate-using-active-directory-based-activation-client)
- [Active Directory 型啟用概觀](/windows/deployment/volume-activation/active-directory-based-activation-overview)

### <a name="multiple-activation-key-mak-activation"></a>多次啟用金鑰 (MAK) 啟用

- [使用 MAK 啟用](/previous-versions/tn-archive/ff793438%28v=technet.10%29)
- [了解 MAK 啟用](/previous-versions/tn-archive/ff793435%28v%3dtechnet.10%29)
- [啟用 MAK 用戶端](/previous-versions/tn-archive/ff793398%28v%3dtechnet.10%29)

### <a name="subscription-activation"></a>訂用帳戶啟用

- [Windows 10 訂用帳戶啟用](/windows/deployment/windows-10-subscription-activation)
- [部署 Windows 10 企業版授權](/windows/deployment/deploy-enterprise-licenses)
- [雲端解決方案提供者中的 Windows 10 企業版 E3](/windows/deployment/windows-10-enterprise-e3-overview)

## <a name="resources-for-troubleshooting-activation-issues"></a>適用於啟用問題疑難排解的資源

下列文章提供有關大量啟用問題疑難排解工具的指導方針和資訊：

- [金鑰管理服務 (KMS) 疑難排解的指導方針](activation-troubleshoot-kms-general.md)
- [用於取得大量啟用資訊的 Slmgr.vbs 選項](activation-slmgr-vbs-options.md)
- [範例：針對未啟用的 ADBA 用戶端進行疑難排解](activation-troubleshoot-adba-clients.md)

下列文章提供解決更多具體啟用問題的指引：

- [解決常見啟用錯誤碼](activation-error-codes.md)
- [KMS 啟用：已知問題](activation-troubleshoot-KMS-issues.md)
- [MAK 啟用：已知問題](activation-troubleshoot-MAK-issues.md)
- [針對 DNS 相關啟用問題進行疑難排解的指導方針](common-troubleshooting-procedures-kms-dns.md)
- [如何重建 Tokens.dat 檔案](activation-rebuild-tokens-dat-file.md)
