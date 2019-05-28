---
title: 檢查清單：將檔案檢測套用至磁碟區或資料夾
description: 本文說明如何將檔案檢測套用至磁碟區或資料夾
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 03325d8a65d88c35f09985223608fc7474a0fde5
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475853"
---
# <a name="checklist---apply-a-file-screen-to-a-volume-or-folder"></a>檢查清單：將檔案檢測套用至磁碟區或資料夾

> 適用於：Windows Server 2019，Windows Server 2016、 Windows Server （半年通道）、 Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2

若要將檔案檢測套用至磁碟區或資料夾，請使用下列清單：
1. 如果您想要透過電子郵件傳送檔案檢測通知或存放裝置報告時，請依照[設定電子郵件通知](configure-email-notifications.md)中的指示設定電子郵件設定。

2. 如果您打算產生 [檔案檢測稽核] 報告，請啟用稽核資料庫中的檔案檢測事件錄製功能。
[設定檔案檢測稽核](configure-file-screen-audit.md)

3. 評定做為檢測規則候選項目的儲存檔案類型。 您可以使用位於 **\[存放裝置報告管理\]** 節點的報告來提供資料 （例如，視需要執行檔案的檔案群組的報表或大型檔案報表以識別佔據大量磁碟空間的檔案。）[產生隨選報告](generate-reports-on-demand.md) 

4. 檢閱預先設定的檔案群組，或建立新的檔案群組，以在組織中強制執行特定檢測原則。 [定義檢測的檔案群組](define-file-groups-for-screening.md)  

5. 檢閱可用檔案檢測範本的內容 (在**檔案檢測管理**，按一下**檔案檢測範本**節點。)[編輯檔案檢測範本內容](edit-file-screen-template-properties.md) 
<br />
 - 或者 -
 <br /> 建立新的檔案檢測範本，以在組織中強制執行存放原則。  [-建立檔案檢測範本](create-file-screen-template.md) 

6. 根據範本在磁碟區或資料夾上建立檔案檢測。 
 [建立檔案檢測](create-file-screen.md)
 
7. 在磁碟區或資料夾的子資料夾中設定檔案檢測例外。 [建立檔案檢測例外](create-file-screen-exception.md) 

8. 排程包含 [檔案檢測稽核] 報告的報告工作，以定期監視檢測活動。
  [排程一組報告](schedule-set-of-reports.md)


> [!NOTE]
> 若要限制磁碟區或資料夾的儲存體，請參閱[檢查清單：將配額套用至磁碟區或資料夾](checklist-apply-file-screen-to-volume-or-folder.md)