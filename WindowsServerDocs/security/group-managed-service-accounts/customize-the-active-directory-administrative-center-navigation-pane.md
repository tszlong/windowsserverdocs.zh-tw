---
title: 自訂 Active Directory 管理中心流覽窗格
description: Windows Server 安全性
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 34a15b768d3143d7083ab62bb9aa537a2c8bdeb4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639796"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>自訂 Active Directory 管理中心流覽窗格

>適用於：Windows Server (半年度管道)、Windows Server 2016

  您可以使用樹狀檢視（類似于 Active Directory 消費者和電腦主控台樹）或使用清單視圖，流覽 Active Directory 管理中心流覽窗格。

 無論您是使用樹狀檢視或清單視圖，都可以隨時自訂您的 Active Directory 管理中心流覽窗格，方法是將不同的容器從本機網域或任何外部網域（除了本機網域的網域，與 \( 本機網域建立信任的網域）新增 \) 至流覽窗格作為個別的節點。 自訂 Active Directory 管理中心流覽窗格可讓您更快速地存取 Active Directory 物件。 如需詳細資訊，請參閱 [管理 Active Directory 管理中心中的不同網域](manage-different-domains-in-active-directory-administrative-center.md)。

 而且，若要進一步自訂瀏覽窗格，您可以重新命名或移除這些手動新增的瀏覽窗格節點、建立這些節點的重複項目，或在瀏覽窗格內上下移動節點。

> [!NOTE]
>  您不可以自訂預設本機網域節點

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>自訂 Active Directory 管理中心瀏覽窗格

1. 在 Active Directory 管理中心流覽窗格中，以滑鼠右鍵 \- 按一下您要修改的節點。 您可以修改節點的位置或名稱，或者您可以建立一個複本。

2. 按一下下列其中一個命令：

   -   **重新命名**

   -   **建立重複節點**

   -   **移除**

   -   **上移**

   -   **下移**

   您可以使用清單視圖來利用最近使用的 \( MRU \) 清單。 當您在此流覽節點中流覽至少一個容器時，MRU 清單會自動出現在流覽節點下。 您也可以展開 Active Directory 管理中心視窗頂端的階層連結列，以查看目前的 MRU 清單。 MRU 清單一律會包含您在特定流覽節點中流覽的最後三個容器。 每次選取特定容器時，這個容器都會新增至 MRU 清單頂端，並從 MRU 清單中移除最後一個容器。



