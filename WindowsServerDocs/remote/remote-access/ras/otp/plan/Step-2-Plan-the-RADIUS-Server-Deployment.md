---
title: 步驟2規劃 RADIUS 伺服器部署
description: 瞭解如何規劃一次性密碼 (OTP) authentication server。
manager: brianlic
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 5658c4c3a323bf9e0df5af8cf57dad85d98fe8dd
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040158"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>步驟2規劃 RADIUS 伺服器部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

部署單一遠端存取服務器之後，請規劃單次密碼 (OTP) authentication server。

|工作|描述|
|----|--------|
|2.1 規劃 RADIUS 伺服器|對於 OTP 驗證服務器，Windows Server 2016 和 Windows Server 2012 中的遠端存取支援支援密碼驗證通訊協定 (PAP) 的任何支援 RADIUS 的 OTP 伺服器。|

## <a name="21-plan-the-radius-server"></a><a name="BKMK_1.1"></a>2.1 規劃 RADIUS 伺服器
規劃用於 OTP 驗證的 RADIUS 伺服器時，請注意下列事項：

-   針對大部分的 OTP 部署類型，您必須將遠端存取服務器設定為 RADIUS 代理程式。 如需詳細資訊，請參閱 OTP 廠商檔。

-   針對所有 OTP 部署，您必須將 Active Directory 使用者與 RADIUS 伺服器同步處理。

-   RADIUS 伺服器不需要是網域成員。

-   當您部署 RADIUS 伺服器時，您會設定共用密碼和 RADIUS 流量的埠號碼。 請記下這些詳細資料;當您設定遠端存取服務器時，這是必要的。

您可以參閱 [測試實驗室指南：示範使用 otp 驗證和 Rsa securid 的 DirectAccess](../../../directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid.md)的範例測試實驗室指南。



