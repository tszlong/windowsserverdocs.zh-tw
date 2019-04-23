---
title: 將頂層類別新增至啟動列 (Macintosh 作業系統)
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869959"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>將頂層類別新增至啟動列 (Macintosh 作業系統)

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以將頂層類別新增至執行 Macintosh 作業系統之電腦上的啟動列。 若要建立可新增頂層類別的啟動列增益集，您可以使用從這個頁面，然後使用說明主題的資訊的組合：將工作和類別新增至啟動列嗎？在  [Windows Server 解決方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
 下列範例顯示如何將啟動列項目指定為 .launchpad 檔案中的頂層類別：  
  
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
  
 為了讓項目成為頂層類別，Category 元素的 Id 屬性必須是 "Microsoft.Launchpad.HomeCategory"。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)