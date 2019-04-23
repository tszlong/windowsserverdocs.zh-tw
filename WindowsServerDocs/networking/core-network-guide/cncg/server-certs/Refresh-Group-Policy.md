---
title: 重新整理群組原則
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dd48297535aafe30e48fe37010d81b279f4c91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863449"
---
# <a name="refresh-group-policy"></a>重新整理群組原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來手動重新整理群組原則，在本機電腦上。 當群組原則重新整理，如果設定憑證自動註冊，而且運作正常，本機電腦會自動註冊憑證授權單位 (CA) 憑證。  
  
> [!NOTE]  
> 當您重新啟動網域成員電腦上，或使用者登入的網域成員電腦時，會自動重新整理群組原則。 此外，會定期重新整理群組原則。 根據預設，這個定期重新整理會使用隨機的位移，最多 30 分鐘每隔 90 分鐘。  
  
中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>若要在本機電腦上重新整理群組原則  
  
1.  在電腦上安裝 NPS，開啟 Windows PowerShell&reg;所使用的圖示在工作列上。  
  
2.  在 Windows PowerShell 提示字元中，輸入**gpupdate**，然後按 ENTER 鍵。  
  


