---
title: 重新整理群組原則
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b9522909960470d9f5f3e183afbd97ab1b919019
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318252"
---
# <a name="refresh-group-policy"></a>重新整理群組原則

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此程式，在本機電腦上手動重新整理群組原則。 重新整理群組原則時，如果已設定憑證自動註冊且正常運作，則會由憑證授權單位單位（CA）將憑證註冊到本機電腦。  
  
> [!NOTE]  
> 當您重新開機網域成員電腦，或當使用者登入網域成員電腦時，群組原則會自動重新整理。 此外，群組原則會定期重新整理。 根據預設，此定期重新整理會每90分鐘執行一次，隨機位移最長為30分鐘。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>若要重新整理本機電腦上的群組原則  
  
1.  在安裝 NPS 的電腦上，使用工作列上的圖示開啟 Windows PowerShell&reg;。  
  
2.  在 Windows PowerShell 命令提示字元中，輸入**gpupdate**，然後按 enter。  
  


