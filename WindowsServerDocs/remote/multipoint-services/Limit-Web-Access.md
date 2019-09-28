---
title: 限制網頁存取
description: 瞭解如何在 MultiPoint 服務中限制使用者存取網際網路
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 044f2fd5-5b87-42bb-ba0d-c06516ac48c8
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 8e12eebd55aa066979bbcbe4d2f3e613b5876a01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395328"
---
# <a name="limit-web-access"></a>限制網頁存取
除了監視個別桌上型電腦上的使用者活動之外，身為系統管理使用者，您可以藉由指示您想要封鎖使用者存取的網站和網站，限制使用者對指定網站的存取權。  
  
## <a name="to-limit-web-access-on-a-station"></a>限制站台上的網頁存取  
  
1. 在 MultiPoint 儀表板的 [ **Web 限制**] 索引標籤上，按一下 [**設定**]。 [Configure Web Limiting]\(設定網頁限制) 頁面隨即開啟， 並列出使用者可存取的網站。  
  
2. 在您要限制網頁存取的使用者站台上，按一下其縮圖。  
  
3. 在 [Selected Item Tasks]\(選取的項目工作) 下，按一下 [Limit web access on this station]\(限制此站台的網頁存取)。 [Configure Web Limiting]\(設定網頁限制) 頁面隨即開啟， 並列出使用者可存取的網站。  
  
4. 若要新增允許的網站，請輸入網址，然後按一下 [新增]。  
  
   > [!NOTE]
   > 例如，輸入 "Contoso.com" 會允許或封鎖相對於 www\.contoso.com 的網站（例如，www\.newpage.contoso.com）。 輸入 "Contoso" 會允許或限制所有 Contoso 相關網站（包括 contoso.com、contoso.uk 等等）。  
  
5. 若要從允許的站台清單移除網址，請按一下您要移除其存取的網址，然後按一下 [移除]。  
  
## <a name="to-limit-web-access-on-all-stations"></a>限制所有站台上的網頁存取  
  
1. 在 MultiPoint 儀表板的 [ **Web 限制**] 索引標籤上，\-按一下 [開始] 下拉式功能表，然後按一下 [**限制所有桌上型電腦上的 Web 存取**]。  
  
   [Configure Web Limiting]\(設定網頁限制) 頁面隨即開啟， 並列出使用者可存取的網站。 執行下列其中一項：  
  
2. 若要新增允許的網站，請按一下 [Allow only these sites]\(僅允許這些網站)，輸入允許的網址，然後按一下 [新增]。  
  
   若要新增您不想讓使用者造訪的網站，請按一下 [**僅禁止這些網站**]，輸入您不想要讓使用者造訪的網址，然後按一下 [**新增**]。  
  
   > [!NOTE]
   > 例如，輸入 "Contoso.com" 會允許或封鎖相對於 www.contoso.com 的網站（例如，www\.newpage.contoso.com）。 輸入 "Contoso" 會允許或限制所有 Contoso 相關網站（包括 contoso.com、contoso.uk 等等）。  
  
3. 若要從允許或拒絕的站台清單移除網址，請選取該網址，然後按一下 [移除]。  
  
## <a name="see-also"></a>另請參閱  
[管理使用者桌面](manage-user-desktops-using-multipoint-dashboard.md)  
