---
title: 啟用永遠離線模式以更快速地存取檔案
description: 如何使用離線檔案的永遠離線模式，以更快的方式存取快取檔案和重新導向的資料夾。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401952"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>啟用永遠離線模式以更快速地存取檔案

>適用於：Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2 和 Windows （半年通道）

本檔說明如何使用離線檔案的永遠離線模式，以更快的方式存取快取檔案和重新導向的資料夾。 永遠離線也會提供較低的頻寬使用量，因為使用者一律會離線工作，即使是透過高速網路連線連接也一樣。

## <a name="prerequisites"></a>必要條件

若要啟用永遠離線模式，您的環境必須符合下列必要條件。

- 已加入網域之用戶端電腦的 Active Directory Domain Services （AD DS）網域。 沒有樹系或網域功能層級需求或架構需求。
- 執行 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的用戶端電腦。 （執行舊版 Windows 的用戶端電腦可能會繼續以非常高速的網路連線轉換成線上模式）。
- 已安裝群組原則管理的電腦。

## <a name="enable-always-offline-mode"></a>啟用永遠離線模式

若要啟用 [永遠離線模式]，請使用群組原則啟用 [設定**低速連結模式]** 原則設定，並將延遲設定為**1** （毫秒）。 這麼做會導致執行 Windows 8 或 Windows Server 2012 的用戶端電腦自動使用永遠離線模式。

>[!NOTE]
>如果網路連線的延遲低於一毫秒，執行 Windows 7、Windows Vista、Windows Server 2008 R2 或 Windows Server 2008 的電腦可能會繼續轉換到線上模式。

1. 開啟**群組原則管理**。
2. 若要選擇性地建立離線檔案設定的新群組原則物件（GPO），請以滑鼠右鍵按一下適當的網域或組織單位（OU），然後選取 [**在這個網域中建立 GPO 並連結到**]。
3. 在主控台樹中，以滑鼠右鍵按一下您要設定離線檔案設定的 GPO，然後選取 [**編輯**]。 **群組原則管理編輯器**隨即出現。
4. 在主控台樹的 [**電腦**設定] 底下，依序展開 [**原則**]、[**系統管理範本**] 和 [**網路**]，然後展開 [**離線檔案**]。
5. 以滑鼠右鍵按一下 [**設定低速連結模式]** ，然後選取 [**編輯**]。 [**設定低速連結模式**] 視窗隨即出現。
6. 選取 [已啟用]。
7. 在 [**選項**] 方塊中，選取 [**顯示**]。 [**顯示內容] 視窗**隨即出現。
8. 在 [**值名稱**] 方塊中，指定您要啟用 [永遠離線] 模式的檔案共用。
9. 若要在所有檔案共用上啟用 [永遠離線模式]，請輸入 **\*** 。
10. 在 [**值**] 方塊中，輸入**延遲 = 1** ，將延遲臨界值設定為1毫秒，然後選取 **[確定]** 。

>[!NOTE]
>根據預設，當處於 Always 離線模式時，Windows 每兩小時會在背景同步處理離線檔案快取中的檔案。 若要變更此值，請使用 [**設定背景同步處理**] 原則設定。

## <a name="more-information"></a>詳細資訊

* [資料夾重新導向、離線檔案和漫遊使用者設定檔總覽](folder-redirection-rup-overview.md)
* [使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)