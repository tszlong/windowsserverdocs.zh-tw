---
title: 限制網頁存取
description: 了解如何限制使用者存取網際網路的 MultiPoint 服務
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 044f2fd5-5b87-42bb-ba0d-c06516ac48c8
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 4274569f76b01c1793f7af7562a87f01ba1bdc07
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446128"
---
# <a name="limit-web-access"></a>限制網頁存取
除了監視個別桌面上的使用者活動，您身為系統管理使用者，可以限制對指定網站的使用者存取表示允許的網站和您要封鎖使用者存取的網站。  
  
## <a name="to-limit-web-access-on-a-station"></a>限制站台上的網頁存取  
  
1. MultiPoint 儀表板上**Web 限制**索引標籤上，按一下**設定**。 [Configure Web Limiting]\(設定網頁限制)  頁面隨即開啟， 並列出使用者可存取的網站。  
  
2. 在您要限制網頁存取的使用者站台上，按一下其縮圖。  
  
3. 在 [Selected Item Tasks]\(選取的項目工作)  下，按一下 [Limit web access on this station]\(限制此站台的網頁存取)  。 [Configure Web Limiting]\(設定網頁限制)  頁面隨即開啟， 並列出使用者可存取的網站。  
  
4. 若要新增允許的網站，請輸入網址，然後按一下 [新增]  。  
  
   > [!NOTE]
   > 例如，輸入"Contoso.com"，允許或封鎖與 www.contoso.com (比方說，www.newpage.contoso.com) 的站台。 輸入"Contoso"將允許或限制所有與 Contoso 相關站台 （包括 contoso.com、 contoso.uk，等等）。  
  
5. 若要從允許的站台清單移除網址，請按一下您要移除其存取的網址，然後按一下 [移除]  。  
  
## <a name="to-limit-web-access-on-all-stations"></a>限制所有站台上的網頁存取  
  
1. MultiPoint 儀表板上**Web 限制**索引標籤上，按一下 開始 下拉式功能表\-向下 功能表，然後按一下**限制網頁存取所有桌面上**。  
  
   [Configure Web Limiting]\(設定網頁限制)  頁面隨即開啟， 並列出使用者可存取的網站。 執行下列其中一項：  
  
2. 若要新增允許的網站，請按一下 [Allow only these sites]\(僅允許這些網站)  ，輸入允許的網址，然後按一下 [新增]  。  
  
   若要新增您不希望使用者前往的網站，請按一下 [Disallow only these sites]\(僅拒絕這些網站)  ，輸入您不希望使用者前往的網址，然後按一下 [新增]  。  
  
   > [!NOTE]
   > 例如，輸入"Contoso.com"，允許或封鎖與 www.contoso.com (比方說，www.newpage.contoso.com) 的站台。 輸入"Contoso"將允許或限制所有與 Contoso 相關站台 （包括 contoso.com、 contoso.uk，等等）。  
  
3. 若要從允許或拒絕的站台清單移除網址，請選取該網址，然後按一下 [移除]  。  
  
## <a name="see-also"></a>另請參閱  
[管理使用者桌面](manage-user-desktops-using-multipoint-dashboard.md)  