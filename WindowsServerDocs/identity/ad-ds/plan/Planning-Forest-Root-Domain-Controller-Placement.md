---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: 規劃設置樹系根網域控制站
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e619d5f5e9cc317f9ba1548d5ed3a32e7bd12c2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402545"
---
# <a name="planning-forest-root-domain-controller-placement"></a>規劃設置樹系根網域控制站

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

必須要有樹系根網域控制站，才能為需要存取其本身以外網域中資源的用戶端建立信任路徑。 將樹系根網域控制站放在中樞位置和裝載資料中心的位置。 如果指定位置中的使用者需要從相同位置的其他網域存取資源，而且資料中心和使用者位置之間的網路可用性不可靠，您可以在位置新增樹系根域控制站，或建立這兩個網域之間的快捷方式信任。 除非您有其他理由將樹系根網域控制站放在該位置，否則在網域之間建立快捷方式信任會比較符合成本效益。  
  
快捷方式信任有助於優化來自任一網域的使用者所提出的驗證要求。 如需有關網域之間的快捷方式信任的詳細資訊，請參閱[瞭解何時建立快捷方式信任一](https://go.microsoft.com/fwlink/?LinkId=107061)文。  
  
如需協助您記載樹系根網域控制站位置的工作表，請參閱[Windows Server 2003 部署套件的工作輔助](https://go.microsoft.com/fwlink/?LinkID=102558)工具、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，然後開啟「網域」控制器位置 "（DSSTOPO_4）。  
  
當您建立樹系根域時，您將需要參考這則資訊。 如需部署樹系根域的詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](https://technet.microsoft.com/library/cc731174.aspx)。  
