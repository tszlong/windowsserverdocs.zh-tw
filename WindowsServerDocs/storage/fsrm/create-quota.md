---
title: 建立配額
description: 本文說明如何以範本為基礎建立配額
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b3513510ef00eec7ea78a3193cf44c25ddb17c7e
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475215"
---
# <a name="create-a-quota"></a>建立配額

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

您可以透過範本或自訂內容建立配額。 下列程序說明如何根據範本建立配額 (建議)。 如果您需要利用自訂內容建立配額，則可將這些內容另存為範本以便日後重複使用。

建立配額時，您可以選擇配額路徑，也就是存放限制所套用到的磁碟區或資料夾。 在指定的配額路徑上，您可以使用範本來建立下列其中一個類型的配額：

-   單一配額，限制整個磁碟區或資料夾空間。
-   自動套用配額，將配額範本指派給資料夾或磁碟區。 以此範本為基礎的配額會自動產生，並套用至所有子資料夾。 如需有關建立自動套用配額的詳細資訊，請參閱[建立自動套用配額](create-auto-apply-quota.md)。


> [!Note]
> 從範本建立專用配額，即可藉由更新範本 (而不是個別配額) 來集中管理您的配額。 然後便可以根據修改的範本將變更套用至所有配額。 這項功能提供一個可進行所有更新作業的中心點，以簡化存放原則變更的實作。

## <a name="to-create-a-quota-that-is-based-on-a-template"></a>若要建立以範本為基礎的配額

1.  按一下 [配額管理]**** 中的 [配額範本]**** 節點。

2.  在 [結果] 窗格中，選取新配額要以之為基礎的範本。

3.  在範本上按滑鼠右鍵，再按一下 **\[從範本建立配額\]** (或從 **\[動作\]** 窗格中選取 **\[從範本建立配額\]**)。 如此會開啟 **\[建立配額\]** 對話方塊，並顯示配額範本摘要內容。

4.  在 **\[配額路徑\]** 下方，輸入配額會套用到的資料夾，或瀏覽至該資料夾。

5.  按一下 **\[在路徑建立配額\]** 選項。 請注意，配額內容將會套用至整個資料夾。

     > [!Note]
     > 若要建立自動套用配額，請按一下 **\[在現有和新子資料夾自動套用範本建立配額\]** 選項。 如需自動套用配額的詳細資訊，請參閱[建立自動套用配額](create-auto-apply-quota.md)。

6.  在 **\[從這個配額範本衍生內容\]** 下方，您在步驟 2 中用來建立新配額的範本已預先選取 (不然也可以從清單選取其他範本)。 請注意，範本的內容會顯示在 **\[配額內容摘要\]** 下方。

7.  按一下 [建立]。

## <a name="additional-references"></a>其他參考

-   [配額管理](quota-management.md)
-   [建立自動套用配額](create-auto-apply-quota.md)
-   [建立配額範本](create-quota-template.md)


