---
title: 安裝增益集
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 12bdaa86cf102ac85d2d2d8f2f2caf492f465dec
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181204"
---
# <a name="install-add-ins"></a>安裝增益集

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以在所有伺服器或用戶端電腦上加入增益集，只要在建立映像之前先安裝增益集即可。 之後，這些增益集就會自動加到所有使用該映像來複寫的電腦上。 您可以執行 .wssx 檔案以安裝增益集或者按照 [SDK 文件](https://go.microsoft.com/fwlink/?LinkID=248648)中各類型增益集的指南安裝個別增益集。 如果使用 .wssx 檔案進行安裝，可透過增益集管理員來解除安裝增益集。 如果您安裝個別檔案，則無法透過增益集管理員管理增益集。

> [!NOTE]
>  任何在伺服器上安裝或下載之軟體 (包括 [儀表板] 索引標籤及健康情況通知) 的當地語系化介面，均應該符合伺服器上所安裝之作業系統的語言。

#### <a name="to-install-an-add-in"></a>若要安裝增益集

1.  (選用) 如果您要使用 .wssx 檔案安裝增益集，請完成下列步驟：

    1.  將 <AddinName \> . onlinebackupaddin.wssx 檔案儲存在參照電腦上。

    2.  按兩下 .wssx 檔案以開啟 [增益集安裝精靈]。

    3.  遵循精靈的指示完成安裝。

2.  (選用) 將個別增益集檔案安裝在適當位置中，如 SDK 中針對每個增益集類型的定義。

## <a name="see-also"></a>另請參閱
 [建立和自訂映射額外的](Creating-and-Customizing-the-Image.md)[自訂](Additional-Customizations.md)[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)