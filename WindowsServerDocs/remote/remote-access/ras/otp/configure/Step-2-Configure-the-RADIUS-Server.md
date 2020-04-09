---
title: 步驟2設定 RADIUS 伺服器
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c2f152d3558e34b6af945937bda61527b0c46dd4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858221"
---
# <a name="step-2-configure-the-radius-server"></a>步驟2設定 RADIUS 伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016

設定遠端存取服務器以支援具有 OTP 支援的 DirectAccess 之前，您必須先設定 RADIUS 伺服器。  
  
|工作|描述|  
|----|--------|  
|[2.1. 設定 RADIUS 軟體發佈權杖](#BKMK_1.1)|在 RADIUS 伺服器上設定軟體發佈權杖。|  
|[2.2. 設定 RADIUS 安全性資訊](#BKMK_1.2)|在 RADIUS 伺服器上，設定要使用的埠和共用密碼。|  
|[2.3 新增 OTP 探查的使用者帳戶](#BKMK_Probe)|在 RADIUS 伺服器上，為 OTP 探查建立新的使用者帳戶。|  
|[2.4 與 Active Directory 同步處理](#BKMK_Active)|在 RADIUS 伺服器上，建立與 Active Directory 帳戶同步處理的使用者帳戶。|  
|[2.5 設定 RADIUS 驗證代理程式](#BKMK_AuthAgent)|將遠端存取服務器設定為 RADIUS 驗證代理程式。|  
  
## <a name="21-configure-the-radius-software-distribution-tokens"></a><a name="BKMK_1.1"></a>2.1 設定 RADIUS 軟體發佈權杖  
RADIUS 伺服器必須設定為使用必要的授權和軟體和/或硬體散發權杖，以供 DirectAccess 使用 OTP。 此程式將專屬於每個 RADIUS 廠商的執行。  
  
## <a name="22-configure-the-radius-security-information"></a><a name="BKMK_1.2"></a>2.2 設定 RADIUS 安全性資訊  
RADIUS 伺服器會針對通訊目的使用 UDP 埠，而每個 RADIUS 廠商都有自己的預設 UDP 埠供傳入和傳出通訊之用。 若要讓 RADIUS 伺服器與遠端存取服務器搭配使用，請確定環境中的所有防火牆都已設定為允許 DirectAccess 和 OTP 伺服器之間的 UDP 流量（如有需要）。  
  
RADIUS 伺服器會使用共用密碼來進行驗證。 為共用密碼設定具有強式密碼的 RADIUS 伺服器，並請注意，當您設定 DirectAccess 伺服器的用戶端電腦設定以搭配使用 DirectAccess 與 OTP 時，將會用到。  
  
## <a name="23-adding-user-account-for-otp-probing"></a><a name="BKMK_Probe"></a>2.3 新增 OTP 探查的使用者帳戶  
在 RADIUS 伺服器上建立名為**DAProbeUser**的新使用者帳戶，並提供密碼**DAProbePass**。  
  
## <a name="24-synchronize-with-active-directory"></a><a name="BKMK_Active"></a>2.4 與 Active Directory 同步處理  
RADIUS 伺服器的使用者帳戶必須與將使用 DirectAccess 搭配 OTP 的 Active Directory 中的使用者對應。  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>同步處理 RADIUS 和 Active Directory 使用者  
  
1.  記錄所有具有 OTP 使用者之 DirectAccess 的 Active Directory 的使用者資訊。  
  
2.  使用廠商專屬程式，在所記錄的 RADIUS 伺服器中建立相同的使用者網域 **\** 網域帳戶。  
  
## <a name="25-configure-the-radius-authentication-agent"></a><a name="BKMK_AuthAgent"></a>2.5 設定 RADIUS 驗證代理程式  
遠端存取服務器必須設定為具有 OTP 執行之 DirectAccess 的 RADIUS 驗證代理程式。 遵循 RADIUS 廠商指示，將遠端存取服務器設定為 RADIUS 驗證代理程式。  
  


