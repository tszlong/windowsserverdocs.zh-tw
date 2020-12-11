---
description: 深入瞭解：網域控制站位置
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: 網域控制站位置
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6885f9cabc9f9f60939e41858f04537cb1bcfcbe
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045116"
---
# <a name="domain-controller-location"></a>網域控制站位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

用戶端會使用網域名稱系統 (DNS) 找出網域控制站來完成作業，例如處理登入要求或在目錄中搜尋已發佈的資源。 網域控制站會在 DNS 中註冊各種記錄，以協助用戶端和其他電腦找到它們。 這些記錄統稱為定位器記錄。

網域控制站也會使用 DNS 找出其他網域控制站，並執行複寫之類的工作。 網域控制站尋找其他網域控制站的程式，與用戶端用來尋找網域控制站的進程相同。



