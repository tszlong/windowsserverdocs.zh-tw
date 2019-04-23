---
title: 步驟 2 設定 RADIUS 伺服器
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c333745d28bdedb9027c1dd46148ac80998f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840869"
---
# <a name="step-2-configure-the-radius-server"></a>步驟 2 設定 RADIUS 伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

設定遠端存取伺服器支援之前 DirectAccess OTP 與支援、 設定 RADIUS 伺服器。  
  
|工作|描述|  
|----|--------|  
|[2.1.設定 RADIUS 軟體發佈權杖](#BKMK_1.1)|RADIUS 伺服器上設定軟體發佈語彙基元。|  
|[2.2.設定 RADIUS 安全性資訊](#BKMK_1.2)|RADIUS 伺服器上設定的連接埠與要使用的共用的密碼。|  
|[2.3 新增使用者帳戶的 OTP 探查](#BKMK_Probe)|RADIUS 伺服器上建立新的使用者帳戶，對於 OTP 探查。|  
|[2.4 與 Active Directory 同步](#BKMK_Active)|RADIUS 伺服器上建立使用者帳戶與 Active Directory 帳戶進行同步處理。|  
|[2.5 設定 RADIUS 驗證代理程式](#BKMK_AuthAgent)|設定遠端存取伺服器做為 RADIUS 驗證代理程式。|  
  
## <a name="BKMK_1.1"></a>2.1 設定 RADIUS 軟體發佈語彙基元  
所需的授權與 DirectAccess OTP 與所使用的軟體和/或硬體發佈語彙基元，必須設定 RADIUS 伺服器。 此程序將會每個 RADIUS 廠商實作特有的。  
  
## <a name="BKMK_1.2"></a>2.2 設定 RADIUS 的安全性資訊  
RADIUS 伺服器會使用 UDP 連接埠進行通訊，以及每個 RADIUS 供應商都有它自己的預設 UDP 連接埠的傳入和傳出通訊。 若要使用的遠端存取伺服器的 RADIUS 伺服器，請確定環境中的所有防火牆都設定為視需要允許 DirectAccess 和 OTP 伺服器之間透過必要的連接埠 UDP 流量。  
  
RADIUS 伺服器會使用共用的密碼進行驗證。 設定 RADIUS 伺服器與共用祕密的強式密碼，並記下 使用 DirectAccess OTP 與設定 DirectAccess 伺服器的用戶端電腦設定，供使用時，將使用此。  
  
## <a name="BKMK_Probe"></a>2.3 新增使用者帳戶的 OTP 探查  
RADIUS 伺服器上建立新的使用者帳戶，稱為**DAProbeUser**並指定其密碼**DAProbePass**。  
  
## <a name="BKMK_Active"></a>2.4 與 Active Directory 同步  
RADIUS 伺服器必須有對應至利用 OTP 會使用 DirectAccess 的 Active Directory 中使用者的使用者帳戶。  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>同步處理 RADIUS 和 Active Directory 使用者  
  
1.  記錄的使用者資訊從 Active Directory 所有 DirectAccess OTP 使用者使用。  
  
2.  使用廠商特定的程序來建立相同的使用者**domain\username** RADIUS 伺服器中所記錄的帳戶。  
  
## <a name="BKMK_AuthAgent"></a>2.5 設定 RADIUS 驗證代理程式  
遠端存取伺服器必須設定為 DirectAccess 與實作 OTP 的 RADIUS 驗證代理程式。 請依照 RADIUS 廠商指示來設定遠端存取伺服器做為 RADIUS 驗證代理程式。  
  


