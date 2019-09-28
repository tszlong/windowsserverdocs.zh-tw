---
title: 設定 HGS 進行 Https 通訊
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f0cbf6a6dc1970499758a6a48bfaadb95c464ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403674"
---
# <a name="configure-hgs-for-https-communications"></a>設定 HGS 進行 HTTPS 通訊

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

根據預設，當您初始化 HGS 伺服器時，它會將 IIS 網站設定為僅限 HTTP 通訊。
所有使用訊息層級加密進行傳輸的機密資料一律都會加密，不過，如果您想要更高層級的安全性，您也可以藉由使用 SSL 憑證設定 HGS 來啟用 HTTPS。

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

