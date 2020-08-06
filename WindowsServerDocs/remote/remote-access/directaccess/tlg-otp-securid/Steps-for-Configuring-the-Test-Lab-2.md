---
title: 使用 OTP 驗證和 RSA SecurID 設定測試實驗室的步驟
description: 本主題屬於測試實驗室指南-示範使用 OTP 驗證的 DirectAccess 和適用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 21a4fe4de5029c009594a0e8dc01dc4570e9e557
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769676"
---
# <a name="steps-for-configuring-the-test-lab-with-otp-authentication-and-rsa-securid"></a>使用 OTP 驗證和 RSA SecurID 設定測試實驗室的步驟

>適用於：Windows Server (半年度管道)、Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從 Homenet 和網際網路子網測試 DirectAccess 連線能力。

在此測試實驗室指南中，您將執行下列步驟，以建立使用 OTP 環境的遠端存取：

-   [步驟1：完成 DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6)設定。 完成[測試實驗室指南：使用混合 IPv4 和 IPv6 來示範 DirectAccess 單一伺服器設定](https://go.microsoft.com/fwlink/p/?LinkId=237004)中的所有步驟。

-   [步驟2：設定 APP1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff)。 以 OTP 憑證範本設定 APP1，以供 EDGE1 使用。

-   [步驟3：設定 DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522)。 確認 DC1 上定義的使用者主體名稱。

-   [步驟7：安裝和設定 RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a)。 安裝和設定 RSA、RADIUS 和 OTP 伺服器，並設定 OTP 的 EDGE1。

-   [步驟11：確認 EDGE1 上的 OTP 健全狀況](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba)。 請確定遠端存取服務器上的 OTP 狀態為狀況良好。

-   [步驟8：從 Homenet 子網測試 DirectAccess 連線能力](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14)。 從 NAT 裝置後方測試 DirectAccess OTP 功能。

-   [步驟10：測試來自網際網路的 DirectAccess 連線能力](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9)。 測試來自網際網路的 DirectAccess 用戶端連線能力。

-   [步驟12：設定設定的快照](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4)。 完成測試實驗室之後，請建立工作 DirectAccess 與 OTP 設定的快照，讓您稍後可以返回它來測試其他案例。



