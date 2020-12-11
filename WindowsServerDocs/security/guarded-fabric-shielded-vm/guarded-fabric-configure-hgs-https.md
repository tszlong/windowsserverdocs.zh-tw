---
description: 深入瞭解：設定 HGS 以進行 HTTPS 通訊
title: 設定 HGS 以進行 HTTPS 通訊
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: c28500d236625410961ef1ee3012001c1d277714
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049886"
---
# <a name="configure-hgs-for-https-communications"></a>設定 HGS 以進行 HTTPS 通訊

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

依預設，當您初始化 HGS 伺服器時，它會設定 IIS 網站僅限 HTTP 通訊。
所有從 HGS 傳輸的機密資料一律會使用訊息層級加密來加密，不過如果您需要更高的安全性層級，您也可以使用 SSL 憑證設定 HGS 來啟用 HTTPS。

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)]

