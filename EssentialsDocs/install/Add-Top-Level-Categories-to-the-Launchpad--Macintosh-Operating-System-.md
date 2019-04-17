---
title: "加入 Launchpad（Macintosh 作業系統）的最上層類型"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ae4eb5943d37b4a9d3b554af28cb425420782cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>加入 Launchpad（Macintosh 作業系統）的最上層類型

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以加入 Launchpad 執行 Macintosh 作業系統的電腦上的最上層的類型。 若要建立加入的最上層分類 Launchpad 增益集，您可以使用從這個頁面及使用方法主題的資訊的組合：新增工作和 Launchpad 分類嗎？在[Windows Server 方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
 下例示範如何，您可以指定.launchpad 檔案中的最上層分類您 Launchpad 項目：  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<LaunchPad resFile="Localizable">  
   <Category id="Microsoft.Launchpad.HomeCategory" name="SampleAddin"  image="SampleImage01.png">  
      <Task id="Microsoft.Launchpad.SampleAddin.Pictures"   
          name="Pictures"       
           src="smb://%ServerAddress%/Pictures"   
         image="SampleImage02.png"/>  
   </Category>  
</LaunchPad>  
```  
  
 將最上層分類的項目，明細項目的編號屬性必須」Microsoft.Launchpad.HomeCategory」。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)