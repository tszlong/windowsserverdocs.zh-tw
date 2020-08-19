---
title: 部署網路原則伺服器
description: 本主題提供適用于 Windows Server 2016 的網路原則伺服器部署內容連結，並包含 NPS 的其他指引連結。
manager: brianlic
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e83e71e87ae9cb442299861125ffb524ef3c1c81
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996843"
---
# <a name="deploy-network-policy-server"></a>部署網路原則伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來取得部署網路原則伺服器的相關資訊。

>[!NOTE]
>如需其他網路原則伺服器檔，您可以使用下列程式庫區段。
>- [開始使用網路原則伺服器](nps-getstart-top.md)
>- [規劃網路原則伺服器](nps-plan-top.md)
>- [管理網路原則伺服器](nps-manage-top.md)

《 Windows Server 2016 核心網路指南》包含規劃和安裝網路原則伺服器 NPS 的一節 \( \) ，本指南中提供的技術可作為在 Active Directory 網域中部署 NPS 的必要條件。 如需詳細資訊，請參閱《 Windows Server 2016 [核心網路指南》](../../core-network-guide/core-network-guide.md#BKMK_deployNPS1)中的「部署 NPS1」一節。

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>部署適用于 VPN 和 802.1 X 存取的 NPS 憑證

如果您想要部署驗證方法，例如 \( \) 需要在 NPS 上使用伺服器憑證的可延伸驗證通訊協定 Eap 和受保護的 eap，您可以使用指南 [部署 802.1 x 有線和無線部署的伺服器憑證](../../core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments.md)來部署 nps 憑證。

## <a name="deploy-nps-for-8021x-wireless-access"></a>部署 802.1 X 無線存取的 NPS

若要部署 NPS 以進行無線存取，您可以使用本指南 [部署密碼型 802.1 x 驗證無線存取](../../core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access.md)。

## <a name="deploy-nps-for-windows-10-vpn-access"></a>部署 Windows 10 VPN 存取的 NPS

您可以 \( \) 針對使用執行 Windows 10 之電腦和裝置的遠端員工，使用 NPS 來處理 Always On 虛擬私人網路 VPN 連線的連線要求。

如需詳細資訊，請參閱 [Windows Server 2016 的遠端存取 ALWAYS ON VPN 部署指南和 Windows 10](../../../remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)。