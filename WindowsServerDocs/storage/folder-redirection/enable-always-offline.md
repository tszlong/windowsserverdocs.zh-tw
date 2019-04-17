---
title: 啟用永遠離線瀏覽模式更快速檔案的存取權
description: 如何使用離線檔案的永遠離線瀏覽模式來存取更快速的快取的檔案和資料夾重新導向。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc54b1e33d09e7f2b9eea01e4f09fb83f13dc1af
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831388"
---
# 啟用永遠離線瀏覽模式更快速檔案的存取權

>適用於： Windows 10，Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2，Windows Server 2016

本文件說明如何使用離線檔案的永遠離線瀏覽模式來存取更快速的快取的檔案和資料夾重新導向。 一律離線也提供較低頻寬使用量，因為使用者一律使用離線，甚至是當它們已經連結透過高速網路連線。

## 必要條件

若要啟用永遠離線瀏覽模式，您的環境必須符合下列先決條件。

- 已加入網域的用戶端電腦使用 Active Directory Domain Services (AD DS) 網域。 沒有樹系或網域功能層級需求或結構描述的需求。
- 執行 Windows 10，Windows 8.1、 Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的用戶端電腦。 （轉換為高速網路連線的線上模式可能會繼續執行較舊版本的 Windows 用戶端電腦）。
- 一部已安裝的群組原則管理。

## 啟用永遠離線瀏覽模式

若要啟用永遠離線瀏覽模式，使用群組原則來啟用**設定低速連結模式**這項原則設定，並將延遲設為**1** （毫秒）。 如此一來，就會導致用戶端電腦執行 Windows 8 或 Windows Server 2012 自動使用永遠離線瀏覽模式。

>[!NOTE]
>執行 Windows 7 電腦，Windows Vista、 Windows Server 2008 R2 或 Windows Server 2008 可能會繼續轉換到線上模式如果電力低於 1 毫秒的網路連線的延遲。

1. 開啟**群組原則管理**。
2. 若要選擇建立新群組原則物件 (GPO) 的離線檔案設定，適當的網域或組織單位 (OU)，以滑鼠右鍵按一下，然後選取**建立這個網域中的 GPO 並連結到此處**。
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下您想以進行離線檔案設定，然後選取 [**編輯**的 GPO。 **群組原則管理編輯器 \]** 隨即顯示。
4. 在主控台樹狀目錄中，在 [**電腦設定**中，展開 [**原則**、**系統管理**範本、 展開**網路**，並展開**離線檔案**。
5. **設定低速連結模式**，以滑鼠右鍵按一下，然後選取 [**編輯**。 **設定低速連結模式**視窗將會出現。
6. 選取 **\[已啟用\]**。
7. 在 [**選項**] 方塊中，選取 [**顯示**。 **顯示內容視窗**將會出現。
8. 在 [**值名稱**] 方塊中，指定您要啟用永遠離線瀏覽模式的檔案共用。
9. 若要啟用永遠離線瀏覽模式上所有的檔案共用，請輸入**\***。
10. 在 [**值**] 方塊中，輸入**延遲 = 1**來將延遲臨界值設定為 1 毫秒，然後選取 **[確定 \**。

>[!NOTE]
>根據預設，在 [永遠離線瀏覽模式中，Windows 會同步處理離線檔案中的快取在背景中的檔案每兩個小時。 若要變更此值，使用**設定背景同步處理**這項原則設定。

## 更多資訊

* [資料夾重新導向、 離線檔案及漫遊使用者設定檔的概觀](folder-redirection-rup-overview.md)
* [部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)