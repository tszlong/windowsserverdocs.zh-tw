---
title: 將頂層類別新增至啟動列 (Macintosh 作業系統)
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 8becc3159dc1f9374b95bbc90b51a44ae0f224e4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624020"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>將頂層類別新增至啟動列 (Macintosh 作業系統)

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以將頂層類別新增至執行 Macintosh 作業系統之電腦上的啟動列。 若要建立可新增頂層類別的啟動列增益集，您可以使用此頁面中的資訊組合，以及從標題為 [如何：將工作和類別新增至啟動列] 的主題中？在 [Windows Server 解決方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)中。

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
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)