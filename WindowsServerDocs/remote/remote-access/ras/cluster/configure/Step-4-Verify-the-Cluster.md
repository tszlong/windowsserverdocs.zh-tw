---
title: 步驟 4： 確認叢集
description: 本主題是部署在 Windows Server 2016 的叢集中的遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b449197265befb7caf7d8d3a5b56accf5c52aef9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821839"
---
# <a name="step-4-verify-the-cluster"></a>步驟 4： 確認叢集

>適用於：Windows Server （半年通道），Windows Server 2016

本主題描述如何確認您已正確設定您的 DirectAccess 叢集部署。  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>若要確認叢集內的內部資源的存取權  
  
1.  將 DirectAccess 用戶端電腦連線到公司網路並取得群組原則。  
  
2.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。  
  
    您應該能夠存取所有公司資源。  
  
3.  在叢集中測試透過每一部伺服器的連線，請關閉，或從外部網路，但其中一部叢集伺服器中斷連線。 在用戶端電腦上嘗試存取公司資源。 重複的測試在不同的叢集伺服器上。  
  
    您應該能夠存取所有公司資源，透過每一部叢集伺服器。  
  


