---
title: 將 HGS 設定為 Https 通訊
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 83529a5bdb4547b9881bb307a8a4cd526552d02c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828999"
---
# <a name="configure-hgs-for-https-communications"></a>將 HGS 設定為 HTTPS 通訊

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

根據預設，當您初始化 HGS 伺服器時它會設定 IIS 網站，僅限 HTTP 通訊。
HGS 來回傳輸的所有機密資料是使用永遠加密訊息層級加密，不過，如果您想要較高層級的安全性也可以藉由設定 HGS 使用 SSL 憑證啟用 HTTPS。

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

