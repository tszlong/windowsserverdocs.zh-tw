---
title: 在 NPS 變更之後驗證設定
description: 在 IP 位址或名稱變更為伺服器後，您可以使用本主題來驗證 Windows Server 2016 網路原則伺服器設定。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ba1f5f494228f6bdba22b300a2336fa6eb37690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405353"
---
# <a name="verify-configuration-after-nps-changes"></a>在 NPS 變更之後驗證設定

>適用於：Windows Server (半年通道)、Windows Server 2016

在 IP 位址或名稱變更為伺服器後，您可以使用本主題來驗證 NPS 設定。

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>在 NPS IP 位址變更之後驗證設定

在某些情況下，您可能需要變更 NPS 或 proxy 的 IP 位址，例如當您將伺服器移到不同的 IP 子網時。 

如果您變更 NPS 或 proxy IP 位址，則必須重新設定 NPS 部署的某些部分。 

使用下列一般指導方針，以協助您確認 IP 位址變更不會在 NPS RADIUS 伺服器和 RADIUS proxy 伺服器的網路上中斷網路存取驗證、授權或帳戶處理。

您必須是系統**管理員**的成員或同等許可權，才能執行這些程式。

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>在 NPS IP 位址變更之後驗證設定

1. 使用 NPS 的新 IP 位址，重新設定所有 RADIUS 用戶端，例如無線存取點和 VPN 伺服器。

2. 如果 NPS 是遠端 RADIUS 伺服器群組的成員，請使用 NPS 的新 IP 位址來重新設定 NPS proxy。

3. 如果您已設定 NPS 使用 SQL Server 記錄，請確認執行 SQL Server 的電腦與 NPS 之間的連線仍然正常運作。

4. 如果您已部署 IPsec 來保護 NPS 與 NPS proxy 或其他伺服器或裝置之間的 RADIUS 流量，請在 [具有 Advanced Security 的 Windows 防火牆] 中重新設定 IPsec 原則或連線安全性規則，以使用 NPS 的新 IP 位址。

5. 如果 NPS 是多重主目錄，而且您已將伺服器設定為系結至特定的網路介面卡，請使用新的 IP 位址重新設定 NPS 通訊埠設定。

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>在 NPS proxy IP 位址變更之後驗證設定

1. 使用 NPS proxy 的新 IP 位址，重新設定所有 RADIUS 用戶端，例如無線存取點和 VPN 伺服器。

2. 如果 NPS proxy 是多重主目錄，而且您已將 proxy 設定為系結至特定的網路介面卡，請使用新的 IP 位址重新設定 NPS 通訊埠設定。

3. 以 proxy 伺服器 IP 位址重新設定所有遠端 RADIUS 伺服器群組的所有成員。 若要完成這項工作，請在每個已將 NPS proxy 設定為 RADIUS 用戶端的 NPS 上執行：

    a. 按兩下 [ **NPS （本機）** ]，按兩下 [ **radius 用戶端和伺服器**]，按一下 [ **radius 用戶端**]，然後在詳細資料窗格中，按兩下您想要變更的 RADIUS 用戶端。

    b. 在 [RADIUS**用戶端**內容] 的 [**位址 \(IP] 或 [DNS\)** ] 中，輸入 NPS proxy 的新 IP 位址。

4. 如果您已設定 NPS proxy 使用 SQL Server 記錄，請確認執行 SQL Server 的電腦與 NPS proxy 之間的連線仍然正常運作。

## <a name="verify-configuration-after-renaming-an-nps"></a>在重新命名 NPS 之後驗證設定

在某些情況下，您可能需要變更 NPS 或 proxy 的名稱，例如當您重新設計伺服器的命名慣例時。

如果您變更 NPS 或 proxy 名稱，則必須重新設定 NPS 部署的某些部分。 

請使用下列一般指導方針，以協助您確認伺服器名稱變更不會中斷網路存取驗證、授權或帳戶處理。

您必須是系統**管理員**的成員或同等許可權，才能執行此程式。

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>若要在 NPS 或 proxy 名稱變更之後驗證設定

1. 如果 NPS 是遠端 RADIUS 伺服器群組的成員，而該群組是以電腦名稱稱而非 IP 位址進行設定，請使用新的 NPS 名稱重新設定遠端 RADIUS 伺服器群組。

2. 如果在 NPS 上部署以憑證為基礎的驗證方法，名稱變更會使伺服器憑證失效。 您可以向憑證授權單位單位（CA）系統管理員要求新的憑證，或者，如果電腦是網域成員電腦，而且您將憑證自動註冊到網域成員，您可以透過自動註冊來重新整理群組原則，以取得新的憑證. 若要重新整理群組原則：

    a. 開啟 [命令提示字元] 或 [Windows PowerShell]。

    b. 輸入 **gpupdate**，然後按 ENTER。


3. 在您擁有新的伺服器憑證之後，請要求 CA 系統管理員撤銷舊的憑證。 

     撤銷舊憑證之後，NPS 會繼續使用它，直到舊憑證過期為止。 根據預設，舊憑證的有效時間最長為一周和10小時。 這段時間可能會因憑證撤銷清單（CRL）到期和傳輸層安全性（TLS）快取時間是否已從其預設值修改而有所不同。 預設 CRL 到期日為一周;預設的 TLS 快取時間逾期為10小時。 

     不過，如果您想要將 NPS 設定為立即使用新憑證，您可以使用新的憑證手動重新設定網路原則。

4. 舊憑證過期之後，NPS 會自動開始使用新的憑證。 

5. 如果您已設定 NPS 使用 SQL Server 記錄，請確認執行 SQL Server 的電腦與 NPS 之間的連線仍然正常運作。

