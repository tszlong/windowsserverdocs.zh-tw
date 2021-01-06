---
title: DirectAccess 不支援的設定
description: 本主題提供 Windows Server 2016 中不支援 DirectAccess 設定的清單。
manager: brianlic
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 827af9315a73bb67cdb017b62150a5fb42c5d69a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946634"
---
# <a name="directaccess-unsupported-configurations"></a>DirectAccess 不支援的設定

>適用於：Windows Server (半年度管道)、Windows Server 2016

在開始部署之前，請先檢查下列不支援的 DirectAccess 設定清單，以避免再次開始部署。

## <a name="file-replication-service-frs-distribution-of-group-policy-objects-sysvol-replications"></a><a name="bkmk_frs"></a>檔案複寫服務 (FRS) 散發群組原則物件 (SYSVOL 複寫) 
請勿在您的網域控制站執行檔案複寫服務的環境中部署 DirectAccess (FRS) ，以 (SYSVOL 複製) 散發群組原則物件。 當您使用 FRS 時，不支援部署 DirectAccess。

如果您有執行 Windows Server 2003 或 Windows Server 2003 R2 的網域控制站，則會使用 FRS。 此外，如果您先前使用的是 Windows 2000 伺服器或 Windows Server 2003 網域控制站，而且您永遠不會將 SYSVOL 複寫從 FRS 遷移至分散式檔案系統 (的 DFS-R) 。

如果您使用 FRS SYSVOL 複寫來部署 DirectAccess，您會有意外刪除包含 DirectAccess 伺服器和用戶端設定資訊之 DirectAccess 群組原則物件的風險。 如果刪除這些物件，您的 DirectAccess 部署將會造成中斷，且使用 DirectAccess 的用戶端電腦將無法連線到您的網路。

如果您打算部署 DirectAccess，您必須使用執行 Windows Server 2003 R2 之後之作業系統的網域控制站，而且必須使用 DFS-R。

如需從 FRS 遷移至 DFS-R 的詳細資訊，請參閱 [SYSVOL 複寫遷移指南： FRS 以 DFS 複寫](../../../storage/dfs-replication/migrate-sysvol-to-dfsr.md)。

## <a name="network-access-protection-for-directaccess-clients"></a><a name="bkmk_nap"></a>DirectAccess 用戶端的網路存取保護
網路存取保護 (NAP) 用來判斷遠端用戶端電腦是否符合 IT 原則，再獲得公司網路的存取權。 NAP 在 Windows Server 2012 R2 中已淘汰，且未包含在 Windows Server 2016 中。 基於這個理由，不建議使用 NAP 開始新部署 DirectAccess。 建議針對 DirectAccess 用戶端的安全性使用不同的端點控制項方法。

## <a name="multisite-support-for-windows-7-clients"></a><a name="bkmk_multi"></a>適用于 Windows 7 用戶端的多網站支援
當在多網站部署中設定 DirectAccess 時，Windows 10 &reg; 、windows &reg; 8.1 和 windows &reg; 8 用戶端都可以連線到最接近的網站。  Windows 7 &reg;  用戶端電腦沒有相同的功能。 適用于 Windows 7 用戶端的網站選取會在原則設定時設定為特定網站，而這些用戶端一律會連線到該指定的網站，無論其位置為何。

## <a name="user-based-access-control"></a><a name="bkmk_user"></a>以使用者為基礎的存取控制
DirectAccess 原則是以電腦為基礎，而不是以使用者為基礎。 不支援指定 DirectAccess 使用者原則來控制公司網路的存取。

## <a name="customizing-directaccess-policy"></a><a name="bkmk_policy"></a>自訂 DirectAccess 原則
您可以使用 DirectAccess 設定向導、遠端存取管理主控台或遠端存取 Windows PowerShell Cmdlet 來設定 DirectAccess。 不支援使用 DirectAccess 設定 Wizard 以外的任何方式來設定 DirectAccess，例如直接修改 DirectAccess 群組原則物件，或手動修改伺服器或用戶端上的預設原則設定。 這些修改可能會導致無法使用的設定。

## <a name="kerbproxy-authentication"></a><a name="bkmk_kerb"></a>KerbProxy authentication
當您使用消費者入門 Wizard 設定 DirectAccess 伺服器時，DirectAccess 伺服器會自動設定為針對電腦和使用者驗證使用 KerbProxy authentication。 基於這個原因，您應該只針對僅 &reg; 部署 Windows 10、Windows 8.1 或 Windows 8 用戶端的單一網站部署使用消費者入門 Wizard。

此外，下列功能不應搭配 KerbProxy authentication 使用：

-   使用外部負載平衡器或 Windows Load Balancer 來進行負載平衡

-   需要智慧卡或一次性密碼 (OTP) 的雙重要素驗證

如果您啟用 KerbProxy authentication，則不支援下列部署計畫：

-   多月臺.

-   適用于 Windows 7 用戶端的 DirectAccess 支援。

-   強制通道。 為確保當您使用強制通道時，不會啟用 KerbProxy authentication，請在執行嚮導時設定下列專案：

    -   啟用強制通道

    -   為 Windows 7 用戶端啟用 DirectAccess

> [!NOTE]
> 針對先前的部署，您應該使用 [Advanced Configuration Wizard]，其使用以憑證為基礎的電腦和使用者驗證的雙通道設定。 如需詳細資訊，請參閱 [使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。

## <a name="using-isatap"></a><a name="bkmk_isa"></a>使用 ISATAP
ISATAP 是一種轉換技術，可在僅 IPv4 的公司網路中提供 IPv6 連線能力。 這僅限於具有單一 DirectAccess 伺服器部署的中小型組織，並允許 DirectAccess 用戶端的遠端系統管理。 如果 ISATAP deployedin 了多站、負載平衡或多網域環境，您必須先將它移除，或將它移至原生 IPv6 部署，再設定 DirectAccess。

## <a name="iphttps-and-one-time-password-otp-endpoint-configuration"></a><a name="bkmk_iphttps"></a> (OTP) 端點設定的 IPHTTPS 和單次密碼
當您使用 IPHTTPS 時，必須在 DirectAccess 伺服器上（而非任何其他裝置，例如負載平衡器）上終止 IPHTTPS 連接。 同樣地，在單次密碼 (OTP) 驗證期間建立的頻外安全通訊端層 (SSL) 連線必須在 DirectAccess 伺服器上終止。 這些連線的端點之間的所有裝置都必須以通過模式設定。

## <a name="force-tunnel-with-otp-authentication"></a><a name="bkmk_ft"></a>使用 OTP 驗證強制通道
請勿使用 OTP 和強制通道的雙重要素驗證部署 DirectAccess 伺服器，否則 OTP 驗證將會失敗。 DirectAccess 伺服器與 DirectAccess 用戶端之間必須有頻外安全通訊端層 (SSL) 連線。 此連線需要豁免才能將流量傳送到 DirectAccess 通道之外。 在強制通道設定中，所有流量都必須流經 DirectAccess 通道，而且在建立通道之後不允許豁免。 因此，不支援在強制通道設定中使用 OTP 驗證。

## <a name="deploying-directaccess-with-a-read-only-domain-controller"></a><a name="bkmk_rodc"></a>使用 Read-Only 網域控制站部署 DirectAccess
DirectAccess 伺服器必須具有讀寫網域控制站的存取權，且 (RODC) 的 Read-Only 網域控制站無法正確運作。

有許多原因需要讀寫網域控制站，包括下列各項：

-   在 DirectAccess 伺服器上，必須有讀寫網域控制站，才能開啟遠端存取 Microsoft Management Console (MMC) 。

-   DirectAccess 伺服器必須同時讀取和寫入 DirectAccess 用戶端和 DirectAccess 伺服器群組原則 (Gpo) 的物件。

-   DirectAccess 伺服器會從主域控制站模擬器明確地讀取和寫入用戶端 GPO， (PDCe) 。

基於這些需求，請勿使用 RODC 部署 DirectAccess。

