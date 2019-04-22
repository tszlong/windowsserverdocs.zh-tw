---
title: 部署網路原則伺服器
description: 本主題提供的 Windows Server 2016 中，網路原則伺服器部署內容的連結，並包含 NPS 的其他指導連結。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8da8951a9c6ed5022c892bbf01b33614d38abc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814839"
---
# <a name="deploy-network-policy-server"></a>部署網路原則伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題針對部署網路原則伺服器的相關資訊。

>[!NOTE]
>如需其他網路原則伺服器文件，您可以使用下列的程式庫各節。  
>- [開始使用網路原則伺服器](nps-getstart-top.md)
>- [規劃網路原則伺服器](nps-plan-top.md)
>- [管理網路原則伺服器](nps-manage-top.md)

Windows Server 2016 核心網路指南包含規劃與安裝網路原則伺服器上的區段\(NPS\)，而且手冊中的技術做為部署在 Active Directory 網域中的 NPS 的必要條件。 如需詳細資訊，請參閱 Windows Server 2016 中的 「 部署 NPS1 」 一節[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)。

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>部署 VPN 和 802.1x 存取的 NPS 憑證

如果您想要部署驗證方法，例如可延伸驗證通訊協定\(EAP\)並受保護的 EAP 需要使用伺服器憑證在您的 NPS 上，您可以使用本指南來部署 NPS 憑證[部署 802.1x 有線和無線部署的伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="deploy-nps-for-8021x-wireless-access"></a>將 NPS 部署 802.1x 無線存取

若要部署 NPS 用於無線存取，您可以使用快速入門[部署密碼型 802.1x 驗證無線存取](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。

## <a name="deploy-nps-for-windows-10-vpn-access"></a>將 NPS 部署為 Windows 10 VPN 存取

您可以使用 NPS 來處理連線要求一律在虛擬私人網路\(VPN\)遠端員工使用執行 Windows 10 電腦和裝置的連線。

如需詳細資訊，請參閱 <<c0> [ 遠端存取一律在 VPN 部署指南的 Windows Server 2016 和 Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)。

