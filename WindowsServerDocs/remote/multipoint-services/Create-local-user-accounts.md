---
title: 建立本機使用者帳戶
description: 瞭解 MultiPoint 服務中的三種使用者帳戶 abou thte
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: b07c88e4961544f5854f6e9d829b8d4b97adf7e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859761"
---
# <a name="create-local-user-accounts"></a>建立本機使用者帳戶
使用 MultiPoint 管理員可以在中建立三種層級的本機使用者帳戶：標準使用者帳戶;具有有限系統管理許可權的 MultiPoint 儀表板使用者;以及完整的系統管理使用者帳戶。  
  
使用下列程式在 MultiPoint 伺服器上建立本機使用者帳戶。 如果您的環境包含多個 MultiPoint 伺服器，而且您想要讓使用者能夠登入任何伺服器上的任何工作站，您必須在每部伺服器上建立本機使用者帳戶。 該安裝程式有一些限制。 在網域環境中，您也可以讓使用者使用其網域帳戶。 如需選項的總覽，請參閱[為您的 Windows MultiPoint 服務環境規劃使用者帳戶](Plan-user-accounts-for-your-MultiPoint-services-environment.md)。  
   
1.  以系統管理員身分登入伺服器，然後開啟 [MultiPoint 管理員]。  
  
2.  按一下 [**使用者**] 索引標籤，然後按一下 [**新增使用者帳戶**]。  
  
    [新增使用者帳戶嚮導] 隨即開啟。  
  
3.  輸入新使用者帳戶的 [帳戶名稱] 和 [密碼]，然後按 **[下一步]** 。  
  
4.  選取您想要建立的使用者帳戶類型：  
  
    -   **標準使用者**-可以登入工作站並執行使用者工作，但無法存取 multipoint 管理員或 Multipoint Server 儀表板，也無法關閉系統。  
  
    -   **MultiPoint 儀表板使用者**-具有有限的系統管理許可權。 儀表板使用者可以開啟 [儀表板] 並執行工作，例如將使用者登出系統或關閉 MultiPoint 伺服器電腦，但使用者沒有 MultiPoint 管理員的存取權。  
  
    -   系統**管理使用者**在 MultiPoint Server 中具有完整的系統管理許可權。 例如，系統管理使用者可以執行 MultiPoint 管理員、新增和刪除使用者、修改系統設定，以及更新驅動程式。  
  
5.  按 **[下一步**]，然後按一下 **[完成]** 以建立使用者帳戶。