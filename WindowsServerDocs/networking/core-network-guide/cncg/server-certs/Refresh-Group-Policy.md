---
title: 重新整理群組原則
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d9f5d38199f8cf3c0ffe46df4cd975cd9c56ff6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="refresh-group-policy"></a>重新整理群組原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以手動更新本機電腦上的 [群組原則使用此程序。 群組原則是重新整理時，如果認證自動授權設定，並且正確運作，會自動註冊憑證來憑證授權單位本機電腦。  
  
> [!NOTE]  
> 當您重新開機網域成員電腦上，或使用者登入成員網域的電腦，會自動重新整理群組原則。 此外，定期更新群組原則。 根據預設，此定期的更新被執行隨機加上最 30 分鐘的每 90 分鐘。  
  
資格在**系統管理員**，或相當於，才能完成此程序最小值。  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>若要在本機電腦上重新整理群組原則  
  
1.  在電腦上安裝 NPS 的位置，請打開 Windows PowerShell&reg;使用工作列上的圖示。  
  
2.  在 Windows PowerShell 命令提示字元中，輸入**gpupdate**，然後按 ENTER 鍵。  
  


