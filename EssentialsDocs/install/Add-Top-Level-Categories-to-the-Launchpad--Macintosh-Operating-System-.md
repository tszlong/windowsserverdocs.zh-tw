---
title: 將頂層類別新增至啟動列 (Macintosh 作業系統)
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5ac16ca5c1b4e01364dc0fa0bfcb5e8dd0c5e54e
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181554"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>將頂層類別新增至啟動列 (Macintosh 作業系統)

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以將頂層類別新增至執行 Macintosh 作業系統之電腦上的啟動列。 若要建立可新增頂層類別的啟動列增益集，您可以使用此頁面中的資訊組合，以及從標題為如何：將工作和類別新增至啟動列的主題。在[Windows Server 解決方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)中。

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
 [建立和自訂映射額外的](Creating-and-Customizing-the-Image.md)[自訂](Additional-Customizations.md)[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)