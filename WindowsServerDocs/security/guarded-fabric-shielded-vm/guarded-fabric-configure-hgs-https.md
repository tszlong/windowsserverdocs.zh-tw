---
title: 設定 HGS 進行 HTTPS 通訊
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: de57a4026a33561760ad36fd78d732352b3aa340
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856841"
---
# <a name="configure-hgs-for-https-communications"></a>設定 HGS 進行 HTTPS 通訊

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

根據預設，當您初始化 HGS 伺服器時，它會將 IIS 網站設定為僅限 HTTP 通訊。
所有使用訊息層級加密進行傳輸的機密資料一律都會加密，不過，如果您想要更高層級的安全性，您也可以藉由使用 SSL 憑證設定 HGS 來啟用 HTTPS。

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

