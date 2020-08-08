---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: 網域控制站位置
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e4736cdad57f375a53ebdb3cb2be035552414139
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941187"
---
# <a name="domain-controller-location"></a>網域控制站位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

用戶端會使用網域名稱系統 (DNS) 尋找網域控制站來完成作業，例如處理登入要求或搜尋目錄中是否有已發佈的資源。 網域控制站會在 DNS 中註冊各種記錄，以協助用戶端和其他電腦找到它們。 這些記錄統稱為定位器記錄。

網域控制站也會使用 DNS 來尋找其他網域控制站，並執行複寫之類的工作。 網域控制站用來尋找其他網域控制站的程式，與用戶端用來尋找網域控制站的進程相同。



