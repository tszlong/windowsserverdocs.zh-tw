---
title: 自訂 Active Directory 管理中心瀏覽窗格
ms.prod: windows-server-threshold
description: Windows Server 安全性
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: e7b1128d93912f724225905bedd38131f8aab0b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854839"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>自訂 Active Directory 管理中心瀏覽窗格

>適用於：Windows Server （半年通道），Windows Server 2016

  使用樹狀檢視中，也就是類似於 Active Directory 使用者和電腦主控台樹狀目錄中，或使用清單檢視，您可以瀏覽 Active Directory 管理中心瀏覽窗格。

 不論您使用 [樹狀] 檢視或 [清單] 檢視，您可以自訂您的 Active Directory 管理中心瀏覽窗格隨時藉由新增各種容器從本機網域或任何外部網域\(亦即，本機網域以外的網域其與本機網域建立的信任\)來瀏覽窗格中，為不同的節點。 自訂 Active Directory 管理中心瀏覽窗格可以更快速地存取 Active Directory 物件。 如需詳細資訊，請參閱 <<c0> [ 在 Active Directory 管理中心管理不同網域](manage-different-domains-in-active-directory-administrative-center.md)。

 而且，若要進一步自訂瀏覽窗格，您可以重新命名或移除這些手動新增的瀏覽窗格節點、建立這些節點的重複項目，或在瀏覽窗格內上下移動節點。

> [!NOTE]
>  您不可以自訂預設本機網域節點

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>若要自訂 Active Directory 管理中心瀏覽窗格

1.  在 [Active Directory 管理中心瀏覽] 窗格中，以滑鼠右鍵\-按一下您想要修改的節點。 您可以修改節點的位置或名稱，或者您可以建立一個複本。

2.  按一下其中一個下列的命令：

    -   **重新命名**

    -   **建立重複的節點**

    -   **移除**

    -   **上移**

    -   **向下移動**

 藉由使用清單檢視中，您可以利用最近使用的\(MRU\)清單。 當您瀏覽這個瀏覽節點中的至少一個容器 MRU 清單會自動顯示瀏覽節點底下。 您也可以藉由展開階層連結列頂端的 [Active Directory 管理中心] 視窗來檢視目前的 MRU 清單。 MRU 清單一律會包含您在特定瀏覽節點內造訪的最後三個容器。 每次選取特定容器時，這個容器都會新增至 MRU 清單頂端，並從 MRU 清單中移除最後一個容器。

  

