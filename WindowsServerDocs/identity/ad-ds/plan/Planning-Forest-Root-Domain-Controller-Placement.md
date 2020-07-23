---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: 規劃設置樹系根網域控制站
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 33dc64cbbfe8eeb7f6593c45d9afc691e75eb0d5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966560"
---
# <a name="planning-forest-root-domain-controller-placement"></a>規劃設置樹系根網域控制站

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

必須要有樹系根網域控制站，才能為需要存取其本身以外網域中資源的用戶端建立信任路徑。 將樹系根網域控制站放在中樞位置和裝載資料中心的位置。 如果指定位置中的使用者需要從相同位置的其他網域存取資源，而且資料中心和使用者位置之間的網路可用性不可靠，您可以在位置新增樹系根網域控制站，或在這兩個網域之間建立快捷方式信任。 除非您有其他理由將樹系根網域控制站放在該位置，否則在網域之間建立快捷方式信任會比較符合成本效益。

快捷方式信任有助於優化來自任一網域的使用者所提出的驗證要求。 如需有關網域之間的快捷方式信任的詳細資訊，請參閱[瞭解何時建立快捷方式信任一](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc754538(v=ws.11))文。

如需協助您記載樹系根網域控制站位置的工作表，請參閱[Windows Server 2003 部署套件的工作輔助工具](https://microsoft.com/download/details.aspx?id=9608)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟「網域控制站位置」（DSSTOPO_4.doc）。

當您建立樹系根域時，您將需要參考這則資訊。 如需部署樹系根域的詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。
