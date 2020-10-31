---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: 規劃設置樹系根網域控制站
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ab34eb19c880fbe22b9da74a6745d9277a564cb4
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071060"
---
# <a name="planning-forest-root-domain-controller-placement"></a>規劃設置樹系根網域控制站

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

需要樹系根網域控制站，才能為需要存取非其專屬網域中資源的用戶端建立信任路徑。 將樹系根網域控制站放在中樞位置和裝載資料中心的位置。 如果指定位置中的使用者需要存取相同位置中其他網域的資源，且資料中心和使用者位置之間的網路可用性不可靠，您可以在該位置新增樹系根網域控制站，或在這兩個網域之間建立快捷方式信任。 除非您有其他原因要將樹系根網域控制站放在該位置，否則在網域之間建立快捷方式信任會更符合成本效益。

快速鍵信任有助於將來自任一網域的使用者所提出的驗證要求優化。 如需網域之間的快捷方式信任的詳細資訊，請參閱文章 [瞭解何時建立快捷方式信任](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc754538(v=ws.11))。

如需協助您記錄樹系根網域控制站放置的工作表，請參閱 [適用于 Windows Server 2003 部署套件的工作輔助程式](https://microsoft.com/download/details.aspx?id=9608)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及開啟 [網域控制站放置] ( # A1) 。

當您建立樹系根域時，您將需要參考此資訊。 如需部署樹系根域的詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。
