---
title: 步驟2設定 RADIUS 伺服器
description: 瞭解如何設定 RADIUS 伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 4dd1ba5d27e3fac32ca5938156d1dc504dd98cfc
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038780"
---
# <a name="step-2-configure-the-radius-server"></a>步驟2設定 RADIUS 伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

設定遠端存取服務器以支援具有 OTP 支援的 DirectAccess 之前，您必須設定 RADIUS 伺服器。

|工作|描述|
|----|--------|
|[2.1. 設定 RADIUS 軟體發佈權杖](#BKMK_1.1)|在 RADIUS 伺服器上設定軟體發佈權杖。|
|[2.2. 設定 RADIUS 安全性資訊](#BKMK_1.2)|在 RADIUS 伺服器上，設定要使用的埠和共用密碼。|
|[2.3 新增 OTP 探查的使用者帳戶](#BKMK_Probe)|在 RADIUS 伺服器上，建立新的使用者帳戶以進行 OTP 探查。|
|[2.4 與 Active Directory 同步處理](#BKMK_Active)|在 RADIUS 伺服器上，建立與 Active Directory 帳戶同步處理的使用者帳戶。|
|[2.5 設定 RADIUS 驗證代理程式](#BKMK_AuthAgent)|將遠端存取服務器設定為 RADIUS 驗證代理程式。|

## <a name="21-configure-the-radius-software-distribution-tokens"></a><a name="BKMK_1.1"></a>2.1 設定 RADIUS 軟體發佈權杖
RADIUS 伺服器必須設定必要的授權和軟體及/或硬體發佈權杖，以供 DirectAccess 搭配 OTP 使用。 此程式將專屬於每個 RADIUS 廠商的實施。

## <a name="22-configure-the-radius-security-information"></a><a name="BKMK_1.2"></a>2.2 設定 RADIUS 安全性資訊
RADIUS 伺服器會使用 UDP 埠進行通訊，而且每個 RADIUS 廠商都有自己的預設 UDP 埠可進行傳入和傳出通訊。 若要讓 RADIUS 伺服器使用遠端存取服務器，請確定環境中的所有防火牆都已設定為允許 DirectAccess 和 OTP 伺服器之間的 UDP 流量在必要的埠上進行。

RADIUS 伺服器會使用共用密碼進行驗證。 使用強式密碼針對共用密碼設定 RADIUS 伺服器，並請注意，這會在設定 DirectAccess 伺服器的用戶端電腦設定以搭配使用 DirectAccess 和 OTP 時使用。

## <a name="23-adding-user-account-for-otp-probing"></a><a name="BKMK_Probe"></a>2.3 新增 OTP 探查的使用者帳戶
在 RADIUS 伺服器上建立名為 **DAProbeUser** 的新使用者帳戶，並提供密碼 **DAProbePass**。

## <a name="24-synchronize-with-active-directory"></a><a name="BKMK_Active"></a>2.4 與 Active Directory 同步處理
RADIUS 伺服器的使用者帳戶必須與將使用 DirectAccess 搭配 OTP 的 Active Directory 中的使用者對應。

#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>同步處理 RADIUS 和 Active Directory 使用者

1.  使用 OTP 使用者記錄所有 DirectAccess 的 Active Directory 中的使用者資訊。

2.  使用廠商特定的程式在所記錄的 RADIUS 伺服器中建立相同的使用者網域 **\ 使用者 \ 使用者 \** 使用者帳戶。

## <a name="25-configure-the-radius-authentication-agent"></a><a name="BKMK_AuthAgent"></a>2.5 設定 RADIUS 驗證代理程式
遠端存取服務器必須設定為具有 OTP 實行的 DirectAccess 的 RADIUS 驗證代理程式。 遵循 RADIUS 廠商的指示，將遠端存取服務器設定為 RADIUS 驗證代理程式。



