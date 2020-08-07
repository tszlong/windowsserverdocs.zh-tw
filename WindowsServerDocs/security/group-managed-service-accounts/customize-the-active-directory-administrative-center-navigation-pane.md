---
title: 自訂 Active Directory 管理中心流覽窗格
description: Windows Server 安全性
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4ecadc09eaa0433458a0224582c3c2ba67dd938f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971475"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>自訂 Active Directory 管理中心流覽窗格

>適用於：Windows Server (半年度管道)、Windows Server 2016

  您可以使用樹狀檢視（類似于 [Active Directory 使用者和電腦] 主控台樹），或使用 [清單] 視圖，流覽 Active Directory 管理中心的流覽窗格。

 無論您使用的是樹狀檢視或清單視圖，都可以隨時自訂您的 Active Directory 管理中心流覽窗格，方法是新增來自本機網域或任何外部網域的各種容器 \( ，也就是與本機網域建立信任的本機網域， \) 做為個別節點的流覽窗格。 自訂 Active Directory 管理中心流覽窗格可讓您更快速地存取 Active Directory 物件。 如需詳細資訊，請參閱[管理 Active Directory 管理中心中的不同網域](manage-different-domains-in-active-directory-administrative-center.md)。

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

   藉由使用清單視圖，您可以利用最近使用過的 \( MRU \) 清單。 當您造訪此流覽節點中的至少一個容器時，MRU 清單會自動出現在流覽節點底下。 您也可以展開 [Active Directory 管理中心] 視窗頂端的階層連結列，以查看目前的 MRU 清單。 MRU 清單一律包含您在特定導覽節點中造訪的最後三個容器。 每次選取特定容器時，這個容器都會新增至 MRU 清單頂端，並從 MRU 清單中移除最後一個容器。



