---
title: 請確認之後 NPS 伺服器變更的設定
description: 若要確認 Windows Server 2016 的網路原則伺服器設定伺服器的 IP 位址或名稱變更之後，您可以使用此主題。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f4d7e003fb037d18c5e5f2036a419383885eaf9e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="verify-configuration-after-nps-server-changes"></a>請確認之後 NPS 伺服器變更的設定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要確認 NPS 伺服器設定伺服器的 IP 位址或名稱變更之後，您可以使用此主題。

## <a name="verify-configuration-after-an-nps-server-ip-address-change"></a>請確認之後 NPS 伺服器 IP 位址變更的設定

可能會有環境，您需要變更 NPS 伺服器或 proxy，例如當您將伺服器移到不同的 IP 子網路的 IP 位址。 

如果您變更 NPS 伺服器或 proxy IP 位址，則必須重新設定您的 NPS 部署的部分。 

使用下列的一般指導方針協助您確認您的 IP 位址變更不會中斷網路存取驗證、授權或計量 RADIUS proxy 伺服器 NPS RADIUS 伺服器，您網路上。

您必須成員的**系統管理員**，或相當於，才能執行這些程序。

### <a name="to-verify-configuration-after-an-nps-server-ip-address-change"></a>若要檢查之後 NPS 設定伺服器的 IP 位址變更

1. 重新設定所有 RADIUS 戶端，例如 wireless 存取點和 VPN 伺服器的新 NPS 伺服器的 IP 位址。

2. 如果 NPS server 的遠端 RADIUS 伺服器群組成員，重新設定 NPS proxy 具有 NPS 伺服器新的 IP 位址。

3. 如果您已設定 NPS 伺服器使用 SQL Server 登入，請確認連接的電腦執行 SQL Server，而且 NPS 伺服器之間仍然正常運作。

4. 如果您有部署保護 RADIUS NPS 伺服器與 NPS proxy 或其他伺服器或裝置間的流量 IPsec，重新 ipsec 或連接安全性規則在 Windows 防火牆使用進階安全性，以使用新的 NPS 伺服器的 IP 位址設定。

5. 如果 NPS 伺服器多重主目錄，而且您已設定繫結至特定網路介面卡的伺服器，重新 NPS 連接埠設定新的 IP 位址。

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>若要檢查之後 NPS 設定 proxy IP 位址變更

1. 重新所有 RADIUS 戶端，例如 wireless 存取點和 VPN 伺服器的 IP 位址 NPS proxy 的新的都設定。

2. 如果 NPS proxy 多重主目錄，且您已設定 proxy 繫結至特定網路介面卡，重新 NPS 連接埠設定新的 IP 位址。

3. 重新使用的 proxy 伺服器的 IP 位址所有遠端 RADIUS 伺服器群組的所有成員的都設定。 若要完成這項工作，每個 NPS 伺服器具有 NPS proxy 設定為 RADIUS client:

    a。 按兩下**NPS（本機）**，按兩下 [ **RADIUS 戶端與伺服器**，按一下 [ **RADIUS 戶端**，然後在詳細資料窗格中，按兩下您想要變更 RADIUS client。

    b。 在 RADIUS client**屬性**，請在**位址 \(IP or DNS\)**，輸入 NPS proxy 的新的 IP 位址。

4. 如果您已設定 NPS proxy 使用 SQL Server 登入，請確認連接的電腦執行 SQL Server，而且 NPS proxy 之間仍然正常運作。

## <a name="verify-configuration-after-renaming-an-nps-server"></a>請確認之後重新命名 NPS 伺服器設定

當您需要變更 NPS 伺服器或 proxy，例如重新命名規格設計伺服器的名稱，可能會環境。

如果您變更 NPS 伺服器或 proxy 名稱，則必須重新設定您的 NPS 部署的部分。 

協助您確認 [伺服器名稱變更不會中斷網路存取驗證，驗證或計量使用下列的一般指導方針。

您必須成員的**系統管理員**，或相當於，才能執行此程序。

### <a name="to-verify-configuration-after-an-nps-server-or-proxy-name-change"></a>若要確認之後 NPS 伺服器或 proxy 名稱變更的設定

1. 如果 NPS server 的遠端 RADIUS 伺服器群組成員，且群組的電腦名稱，而非 IP 位址設定，請重新設定遠端 RADIUS 伺服器群組具有 NPS 伺服器名稱。

2. 如果憑證架構的驗證方法在 NPS 伺服器部署，該名稱變更失效伺服器的憑證。 您可以從「憑證授權單位系統管理員要求一個新的憑證，或如果您的電腦位於網域成員電腦及您的網域成員註冊憑證，您可以重新整理群組原則」來取得新的憑證透過自動註冊。 重新整理群組原則中︰

    a。 命令提示字元」或「Windows PowerShell 開放。

    b。 輸入**gpupdate**，然後按 ENTER 鍵。


3. 新的伺服器憑證之後，請要求 CA 系統管理員撤銷舊的憑證。 

     舊憑證已被撤銷後，NPS 持續使用它，直到結束舊的憑證。 根據預設，舊的憑證維持有效的一星期 10 小時的時間上限。 這段期間，可能會不同而定是否憑證撤銷清單 (CRL) 到期與傳輸層級的安全性 (TLS) 快取時間到期經過修改的預設值。 預設 CRL 到期是一星期;預設 TLS 快取到期是 10 小時的時間。 

     如果您想要設定 NPS 立即使用新的憑證，但是您以手動方式可以重新設定新的憑證的網路原則。

4. 舊的憑證到期之後，NPS 自動開始使用新的憑證。 

5. 如果您已設定 NPS 伺服器使用 SQL Server 登入，請確認連接的電腦執行 SQL Server，而且 NPS 伺服器之間仍然正常運作。

