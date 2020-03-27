---
title: 部署網路原則伺服器
description: 本主題提供 Windows Server 2016 的網路原則伺服器部署內容連結，並包含 NPS 其他指引的連結。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e91f5ce22bcd48e486052ecf54a13617301a058b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316155"
---
# <a name="deploy-network-policy-server"></a>部署網路原則伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題以取得部署網路原則伺服器的相關資訊。

>[!NOTE]
>如需其他網路原則伺服器檔，您可以使用下列程式庫區段。  
>- [網路原則伺服器的消費者入門](nps-getstart-top.md)
>- [規劃網路原則伺服器](nps-plan-top.md)
>- [管理網路原則伺服器](nps-manage-top.md)

《 Windows Server 2016 核心網路指南》包含規劃和安裝網路原則伺服器 \(NPS\)的一節，而本指南所提供的技術是在 Active Directory 網域中部署 NPS 的必要條件。 如需詳細資訊，請參閱《 Windows Server 2016[核心網路指南》](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)中的「部署 NPS1」一節。

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>部署適用于 VPN 和 802.1 X 存取的 NPS 憑證

如果您想要部署驗證方法（例如可延伸的驗證通訊協定） \(EAP\) 和受保護的 EAP （需要在您的 NPS 上使用伺服器憑證），您可以使用[部署伺服器憑證以進行 802.1 x 有線和無線部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)指南來部署 NPS 憑證。

## <a name="deploy-nps-for-8021x-wireless-access"></a>部署適用于 802.1 X 無線存取的 NPS

若要部署 NPS 以進行無線存取，您可以使用指南[部署以密碼為基礎的 802.1 x 驗證無線存取](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。

## <a name="deploy-nps-for-windows-10-vpn-access"></a>部署適用于 Windows 10 VPN 存取的 NPS

您可以使用 NPS 為使用執行 Windows 10 之電腦和裝置的遠端員工，處理 Always On 虛擬私人網路 \(VPN\) 連線的連線要求。

如需詳細資訊，請參閱[Windows Server 2016 和 windows 10 的遠端存取 ALWAYS ON VPN 部署指南](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)。

