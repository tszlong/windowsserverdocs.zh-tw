---
title: "檔案管理工作"
description: "本文說明自動化檔案管理工作的流程"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e83d0b79117144d42a0aff748f482f3c181cb300
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="file-management-tasks"></a>檔案管理工作

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

檔案管理工作會自動化在伺服器上找到檔案子集後套用簡易命令的流程。 您可以將這些工作排程為定期發生，以降低重複的成本。 您可以透過下列任何屬性，定義要由檔案管理工作處理的檔案：

-   位置
-   分類屬性
-   建立時間
-   修改時間
-   上次存取時間

也可以將檔管理工作設定為將任何即將套用至檔案的原則通知其檔案擁有者。

> [!Note]
> 個別檔案管理工作是依照獨立排程來執行。

<br />
本節包含下列主題：

-   [建立檔案到期工作](create-file-expiration-task.md)
-   [建立自訂檔案管理工作](create-custom-file-management-task.md)

> [!Note]
> 若要設定電子郵件通知和某些報告功能，您必須先設定檔案伺服器資源管理員的一般選項。

## <a name="see-also"></a>請參閱

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)


