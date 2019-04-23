---
title: 確認 NPS 變更後的組態
description: 您可以使用本主題，以確認 Windows Server 2016 的網路原則伺服器設定之後變更伺服器的 IP 位址或名稱。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 144e414e32d413e4863b90ada671753155bc96d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880669"
---
# <a name="verify-configuration-after-nps-changes"></a>確認 NPS 變更後的組態

>適用於：Windows Server （半年通道），Windows Server 2016

若要確認 NPS 設定伺服器的 IP 位址或名稱變更之後，您可以使用本主題。

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>確認 NPS IP 位址變更之後的組態

有可能在某些情況下您要變更 NPS 或 proxy 時，例如當您將伺服器移到不同的 IP 子網路的 IP 位址。 

如果您變更 NPS 或 proxy IP 位址，就必須重新設定您的 NPS 部署的部分。 

您可以使用下列一般指導方針，協助您確認變更 IP 位址不會中斷網路存取驗證、 授權或 NPS RADIUS 伺服器與 RADIUS proxy 伺服器在網路上的計量。

您必須是隸屬**系統管理員**，或同等權限，才能執行這些程序。

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>若要驗證 NPS IP 位址變更之後的組態

1. 重新設定所有的 RADIUS 用戶端，例如無線存取點與 VPN 伺服器，NPS 的新 IP 位址。

2. 如果 NPS 是遠端 RADIUS 伺服器群組的成員，重新設定 NPS proxy 與 NPS 的新 IP 位址。

3. 如果您已設定為使用 SQL Server 記錄 NPS，請確認執行 SQL 伺服器與 NPS 伺服器的電腦之間的連線仍然運作正常。

4. 如果您已部署 IPsec 來保護您的 NPS 與 NPS proxy 或其他伺服器或裝置之間的 RADIUS 流量，重新設定 IPsec 原則或連線安全性規則，在 具有進階安全性，以使用新的 IP 位址，NPS 的 Windows 防火牆。

5. 如果 NPS 是多重主目錄，而且您已設定要繫結至特定網路介面卡的伺服器，重新設定 NPS 使用新的 IP 位址的通訊埠設定。

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>若要確認 NPS 之後設定 proxy IP 位址會變更

1. 重新設定所有的 RADIUS 用戶端，例如無線存取點與 VPN 伺服器，NPS proxy 的新 IP 位址。

2. 如果 NPS proxy 是多重主目錄，而且您已經設定要繫結至特定網路介面卡的 proxy，請重新設定 NPS 使用新的 IP 位址的通訊埠設定。

3. 重新設定 proxy 伺服器 IP 位址與所有遠端 RADIUS 伺服器群組的所有成員。 若要完成這項工作，在每個已設定為 RADIUS 用戶端的 NPS proxy 的 NPS:

    a. 按兩下**NPS （本機）**，按兩下**RADIUS 用戶端和伺服器**，按一下  **RADIUS 用戶端**，然後在 詳細資料 窗格中，按兩下 RADIUS 用戶端，您想要變更。

    b. 在 RADIUS 用戶端**屬性**，請在**地址\(IP 或 DNS\)**，輸入新的 IP 位址，NPS proxy。

4. 如果您已設定為使用 SQL Server 記錄 NPS proxy，請確認執行 SQL Server 和 NPS proxy 的電腦之間的連線仍然運作正常。

## <a name="verify-configuration-after-renaming-an-nps"></a>驗證之後重新命名的 NPS 設定

當您需要變更的 NPS 或 proxy 時，例如當您重新命名慣例設計為您的伺服器名稱時，則可能會在某些情況下。

如果您變更 NPS 或 proxy 的名稱，就必須重新設定您的 NPS 部署的部分。 

您可以使用下列一般指導方針來協助您確認伺服器名稱變更不會中斷網路存取驗證、 授權或帳戶處理。

您必須是隸屬**系統管理員**，或同等權限，才能執行此程序。

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>若要驗證 NPS 或 proxy 的名稱變更之後的組態

1. 如果 NPS 是遠端 RADIUS 伺服器群組的成員和群組設定電腦名稱，而不是 IP 位址，重新設定遠端 RADIUS 伺服器群組，以新的 NPS 名稱。

2. 如果憑證為基礎的驗證方法部署在 NPS 中，名稱變更會導致無效的伺服器憑證。 您可以要求新憑證從憑證授權單位 (CA) 系統管理員，或如果電腦是網域成員電腦和您的網域成員的自動註冊憑證，您可以重新整理群組原則 」 來取得新的憑證，透過自動註冊. 若要重新整理群組原則：

    a. 開啟命令提示字元] 或 [Windows PowerShell。

    b. 輸入 **gpupdate**，然後按 ENTER。


3. 新的伺服器憑證之後，請要求 CA 系統管理員撤銷舊憑證。 

     撤銷舊憑證之後，NPS 會繼續使用它，直到舊的憑證到期為止。 根據預設，舊的憑證會保持有效的一週 10 小時的時間上限。 這段時間可能取決於是否在憑證撤銷清單 (CRL) 到期與傳輸層安全性 (TLS) 快取時間到期已修改的預設值不同。 預設 CRL 過期狀態為一週;預設 TLS 快取到期日為 10 小時的時間。 

     如果您想要設定 NPS 立即使用新的憑證，不過，您以手動方式可以重新設定網路原則與新的憑證。

4. 舊的憑證過期後，NPS 便會自動開始使用新的憑證。 

5. 如果您已設定為使用 SQL Server 記錄 NPS，請確認執行 SQL 伺服器與 NPS 伺服器的電腦之間的連線仍然運作正常。

