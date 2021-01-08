---
title: 重新整理群組原則
description: 瞭解如何在本機電腦上手動更新群組原則。
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: db578b1c36548106fa2995469a7e302d4d029901
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038538"
---
# <a name="refresh-group-policy"></a>重新整理群組原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，手動重新整理本機電腦上的群組原則。 當群組原則重新整理時，如果憑證自動註冊已設定且正常運作，則憑證授權單位單位 (CA) 會自動註冊本機電腦。

> [!NOTE]
> 當您重新開機網域成員電腦，或當使用者登入網域成員電腦時，群組原則會自動重新整理。 此外，群組原則會定期重新整理。 依預設，此定期重新整理會每隔90分鐘執行一次，隨機位移為最多30分鐘。

若要完成此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-refresh-group-policy-on-the-local-computer"></a>重新整理本機電腦上的群組原則

1.  在安裝 NPS 的電腦上， &reg; 使用工作列上的圖示開啟 Windows PowerShell。

2.  在 Windows PowerShell 提示字元中，輸入 **gpupdate**，然後按 enter。



