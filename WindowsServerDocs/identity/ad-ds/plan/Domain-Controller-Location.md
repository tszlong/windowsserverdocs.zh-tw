---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: 網域控制站位置
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ada91f7205ba12e4d37a02e3503647c9560ccdd8
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939268"
---
# <a name="domain-controller-location"></a>網域控制站位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

用戶端會使用網域名稱系統 (DNS) 找出網域控制站來完成作業，例如處理登入要求或在目錄中搜尋已發佈的資源。 網域控制站會在 DNS 中註冊各種記錄，以協助用戶端和其他電腦找到它們。 這些記錄統稱為定位器記錄。

網域控制站也會使用 DNS 找出其他網域控制站，並執行複寫之類的工作。 網域控制站尋找其他網域控制站的程式，與用戶端用來尋找網域控制站的進程相同。



