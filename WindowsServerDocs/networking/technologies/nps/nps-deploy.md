---
title: 部署原則的網路伺服器
description: 本主題提供 Windows Server 2016 的網路原則伺服器部署 content 連結，並包含 NPS 設計的額外指導的連結。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daf62e2a593014e16bcb5fd16542b2e9c75b9c1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-policy-server"></a>部署原則的網路伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題適用於部署的網路原則伺服器的相關資訊。

>[!NOTE]
>其他網路原則伺服器文件，您可以使用下列的媒體櫃區段。  
>- [開始使用的網路原則伺服器](nps-getstart-top.md)
>- [規劃網路原則伺服器](nps-plan-top.md)
>- [管理網路原則伺服器](nps-manage-top.md)

Windows Server 2016 核心網路指南包含區段規劃並安裝網路原則伺服器 \(NPS\)，並在節目表中顯示的技術作為部署 NPS Active Directory 網域中的必要條件。 如需詳細資訊，查看 Windows Server 2016 中的區段「部署 NPS1」[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)。

## <a name="deploy-nps-server-certificates-for-vpn-and-8021x-access"></a>NPS 伺服器的憑證部署 VPN 和 802.1 X 存取

如果您想要部署像是延伸驗證通訊協定 \(EAP\) 和保護 EAP 需要使用伺服器上的憑證 NPS 伺服器的驗證方法，您可以將 NPS 伺服器的憑證部署本指南使用[適用於 802.1 X 的有線和無線部署部署伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="deploy-nps-for-8021x-wireless-access"></a>NPS 部署 802.1 X Wireless 存取

Wireless 存取部署 NPS，您可以使用快速入門[架構部署密碼 802.1 X 驗證 Wireless 存取](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。

## <a name="deploy-nps-for-windows-10-vpn-access"></a>NPS 部署 Windows 10 VPN 存取

您可以使用 NPS 處理連接遠端電腦與執行 Windows 10 的裝置使用的員工一律上 Virtual 私人網路 \(VPN\) 連接的要求。

如需詳細資訊，請查看[遠端存取永遠上 VPN 部署節目表適用於 Windows Server 2016 和 Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)。

