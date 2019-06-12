---
title: 啟用永遠離線模式更快速地存取檔案
description: 如何使用永遠離線模式的離線檔案，以提供更快速地存取快取的檔案和資料夾重新導向。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: ddf6a816e417c2eddff090df8dba841a894a3255
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447668"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>啟用永遠離線模式更快速地存取檔案

>適用於：Windows 10，Windows 8、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012、 Windows Server 2012 R2 和 Windows （半年通道）

本文件說明如何使用永遠離線模式的離線檔案，以提供更快速地存取快取的檔案和資料夾重新導向。 永遠離線也提供較低的頻寬使用量，因為使用者會永遠離線工作，即使當他們連線透過高速網路連線。

## <a name="prerequisites"></a>先決條件

若要啟用永遠離線模式，您的環境必須符合下列必要條件。

- 與用戶端電腦加入網域的 Active Directory 網域服務 (AD DS) 網域。 沒有任何樹系或網域功能等級需求或結構描述的需求。
- 執行 Windows 10，Windows 8.1，Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的用戶端電腦。 （轉換為線上模式非常高速的網路連線可能會繼續執行舊版 Windows 的用戶端電腦）。
- 具有安裝群組原則管理的電腦。

## <a name="enable-always-offline-mode"></a>啟用永遠離線模式

若要啟用永遠離線模式，請使用群組原則來啟用**設定低速連結模式**原則設定，並將延遲設定為**1** （毫秒）。 這樣做會讓執行 Windows 8 或 Windows Server 2012，會自動使用永遠離線模式的用戶端電腦。

>[!NOTE]
>執行 Windows 7 的電腦，Windows Vista、 Windows Server 2008 R2 或 Windows Server 2008 可能會繼續轉換為線上模式如果網路連線的延遲低於一毫秒。

1. 開啟**群組原則管理**。
2. 若要選擇性地建立新群組原則物件 (GPO) 的離線檔案設定，以滑鼠右鍵按一下適當的網域或組織單位 (OU)，然後按**在這個網域中建立 GPO 並連結到**。
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下您要設定離線檔案設定，然後選取的 GPO**編輯**。 **群組原則管理編輯器**隨即出現。
4. 在主控台樹狀目錄中，在**電腦組態**，展開**原則**，展開**系統管理範本**，展開**網路**，展開 **離線檔案**。
5. 以滑鼠右鍵按一下**設定低速連結模式**，然後選取**編輯**。 **設定低速連結模式**視窗會出現。
6. 選取 [已啟用]  。
7. 在 **選項**方塊中，選取**顯示**。 **視窗中顯示內容**會出現。
8. 在 **值名稱**方塊中，指定您要啟用永遠離線模式的檔案共用。
9. 若要啟用永遠離線模式的所有檔案共用上，輸入 * *\\* * *。
10. 在 **值**方塊中，輸入**延遲 = 1**毫秒，以設定延遲臨界值，然後選取**確定**。

>[!NOTE]
>根據預設，在 永遠離線模式中，Windows 會同步處理離線檔案快取，在背景中的檔案每隔兩小時。 若要變更此值，請使用**設定背景同步處理**原則設定。

## <a name="more-information"></a>詳細資訊

* [資料夾重新導向、 離線檔案和漫遊使用者設定檔的概觀](folder-redirection-rup-overview.md)
* [部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)