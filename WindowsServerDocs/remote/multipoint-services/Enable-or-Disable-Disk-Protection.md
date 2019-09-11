---
title: 啟用或停用磁碟保護
description: 瞭解如何使用 MultiPoint 服務的磁片保護
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00aba4c4-0244-4b39-8c85-c46fd96e1d6a
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: d037b0843f5ba50c98e0d6e7cb10836c8d6fa23a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871755"
---
# <a name="enable-or-disable-disk-protection"></a>啟用或停用磁碟保護
[磁碟保護] 功能可讓您在每次重新啟動系統時，都能將 MultiPoint 服務系統重設為指定的狀態。 透過 [磁碟保護]，使用者可以暫時變更 MultiPoint 服務系統，這些變更將在重新啟動伺服器時被捨棄。 重新開機伺服器時將捨棄的變更範例包括個人化使用者的設定檔、儲存檔案、變更設定，或安裝應用程式。  
  
## <a name="enable-disk-protection"></a>啟用磁片保護  
  
1.  在 [MultiPoint 管理員] 中，按一下 [**首頁**] 索引標籤，然後在 [電腦名稱稱] 和 [工作]**底下，按一下**[**啟用磁片保護**]。  
  
2.  檢視資訊，然後按一下 [確定]。  
  
系統重新啟動之後，對系統所做的任何變更 (包括安裝的新應用程式) 都會在隨後每次重新啟動時被捨棄。  
  
## <a name="disable-disk-protection"></a>停用磁片保護  
  
1.  在 [MultiPoint 管理員] 中，按一下 [**首頁**] 索引標籤，然後在 [電腦名稱稱] 和 [工作]**底下，按一下**[停用**磁片保護**]。  
  
2.  檢視資訊，然後按一下 [確定]。  
  
系統重新啟動之後，對系統所做的任何變更 (包括安裝在伺服器上的應用程式) 會是永久的，而且下次重新啟動系統時將不會被捨棄。  
  
