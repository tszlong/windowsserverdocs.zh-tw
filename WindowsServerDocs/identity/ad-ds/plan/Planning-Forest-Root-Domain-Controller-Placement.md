---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: 規劃設置樹系根網域控制站
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57eafcc884a827d98c249e2da0c0af6888abc5b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823639"
---
# <a name="planning-forest-root-domain-controller-placement"></a>規劃設置樹系根網域控制站

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立需要存取非自己的網域中資源的用戶端的信任路徑所需的樹系根網域控制站。 在中樞的位置，以及在裝載資料中心的位置，請將樹系根網域控制站。 如果指定的位置中的使用者需要從其他網域中相同的位置，存取資源，而且不可靠的資料中心與使用者的位置之間的網路可用性，您可以將樹系根網域控制站新增的位置或建立兩個網域之間的捷徑信任。 它會更符合成本效益建立捷徑信任網域之間，除非您有其他原因，若要將樹系根網域控制站放在該位置。  
  
捷徑信任來最佳化任一網域的使用者所提出的驗證要求的說明。 如需網域之間的捷徑信任的詳細資訊，請參閱文章[了解何時建立捷徑信任](https://go.microsoft.com/fwlink/?LinkId=107061)。  
  
可協助您記載您的樹系根網域控制站的建置工作表，請參閱 <<c0> [ 工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip然後開啟 「 設置網域控制站 」 (DSSTOPO_4.doc)。  
  
您必須參考此資訊，當您建立樹系根網域。 如需部署樹系根網域的詳細資訊，請參閱 <<c0> [ 部署 Windows Server 2008 樹系根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
