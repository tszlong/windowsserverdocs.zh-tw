---
title: 建立本機使用者帳戶
description: 了解 abou 修改三種類型的 MultiPoint 服務中的使用者帳戶
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: e2e5a6c79e6dcd603d19ca868df1d11fce2bf746
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823669"
---
# <a name="create-local-user-accounts"></a>建立本機使用者帳戶
使用 MultiPoint 管理員，您可以建立三個本機使用者帳戶層級：標準使用者帳戶;MultiPoint 儀表板的使用者，具有有限的系統管理權限;完整的系統管理使用者帳戶和。  
  
使用下列程序來建立 MultiPoint 伺服器上的本機使用者帳戶。 如果您的環境包含多個 MultiPoint 伺服器，而且您想讓使用者能夠登入的任何伺服器上的任何站台，您需要在每個伺服器上建立本機使用者帳戶。 安裝程式會有一些限制。 在網域環境中，您也可以讓使用者使用他們的網域帳戶。 如需選項的概觀，請參閱 <<c0> [ 規劃您的 Windows MultiPoint 服務環境的使用者帳戶](Plan-user-accounts-for-your-MultiPoint-services-environment.md)。  
   
1.  登入伺服器，身為管理員，並開啟 MultiPoint 管理員。  
  
2.  按一下 **使用者**索引標籤，然後再按一下**加入使用者帳戶**。  
  
    新增使用者帳戶精靈 隨即開啟。  
  
3.  輸入新的使用者帳戶的帳戶名稱和密碼，然後按一下**下一步**。  
  
4.  選取您想要建立的使用者帳戶類型：  
  
    -   **標準使用者**-可以登入站台，並執行使用者工作，但不具有存取權 MultiPoint 管理員] 或 [MultiPoint Server 儀表板中，且無法關閉系統。  
  
    -   **MultiPoint 儀表板使用者**-有限系統管理權限。 儀表板使用者可以開啟儀表板，並執行工作，例如記錄使用者登出系統，或關閉 MultiPoint Server 的電腦，但使用者沒有存取權 MultiPoint 管理員。  
  
    -   **系統管理使用者**具有 MultiPoint Server 中的完整系統管理權限。 比方說，系統管理使用者可以執行 MultiPoint 管理員、 加入和刪除使用者、 修改系統設定，以及更新驅動程式。  
  
5.  按一下 **下一步**，然後按一下**完成**建立使用者帳戶。