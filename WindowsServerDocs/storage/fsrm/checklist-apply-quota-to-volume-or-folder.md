---
title: 將配額套用至磁碟區或資料夾
description: 本文說明如何將儲存配額套用至磁碟區或資料夾
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 44eea3c3802e261239db9bf8350777e1b83fb9c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394328"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>檢查清單：將配額套用至磁碟區或資料夾

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server （半年通道）

1. 如果您想要透過電子郵件傳送閾值通知或存放裝置報告，請設定電子郵件設定。 [設定電子郵件通知](configure-email-notifications.md)

2. 評定磁碟區或資料夾的儲存空間需求。 您可以使用位於 **\[存放裝置報告管理\]** 節點的報告來提供資料 (例如，視需要執行 [檔案: 依擁有者列表] 報告，以找出使用大量磁碟空間的使用者)。[產生隨選報告](generate-reports-on-demand.md)

3. 檢閱可用的預先設定配額範本 （在 [**配額管理**] 中，按一下 [**配額範本**] 節點）。[編輯配額範本屬性](edit-quota-template-properties.md) 
<br />\- 或者 - <br /> 建立新的配額範本，以在組織中強制執行存放原則。 [建立配額範本](create-quota-template.md)

4. 根據範本在磁碟區或資料夾上建立配額。  
 [建立配額](create-quota.md) <br /> \- 或者 - <br /> 建立自動套用配額以自動針對磁碟區或資料夾的子資料夾產生配額。 [建立自動套用配額](create-auto-apply-quota.md)

6. 排程包含 [配額使用量] 報告的報告工作，以定期監視配額使用量。 [排程一組報告](schedule-set-of-reports.md)

> [!Note]
> 如果您想要檢測磁碟區或資料夾上的檔案，請參閱[檢查清單：將檔案檢測套用至磁碟區或資料夾](checklist-apply-file-screen-to-volume-or-folder.md)。











