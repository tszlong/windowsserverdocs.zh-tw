---
title: 設定測試實驗室的步驟
description: 本主題是測試實驗室指南-使用 OTP 驗證和 RSA SecurID for Windows Server 2016 的示範 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 991f06568a16f7e8bcce3bc6c86eef3467ea31e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826289"
---
# <a name="steps-for-configuring-the-test-lab"></a>設定測試實驗室的步驟

>適用於：Windows Server （半年通道），Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、 設定遠端存取伺服器和用戶端，以及測試從家用網路和網際網路的子網路的 DirectAccess 連線。  
  
在這個測試實驗室指南中，您將建置 OTP 環境的遠端存取執行下列步驟：  
  
-   [步驟 1:完成 DirectAccess 設定](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6)。 完成中的所有步驟[測試實驗室指南：示範 DirectAccess 單一伺服器安裝在混合的 IPv4 和 IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004)。  
  
-   [步驟 2:設定 APP1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff)。 設定 APP1 的 OTP 憑證範本，以供 EDGE1。  
  
-   [步驟 3:設定 DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522)。 請確認 DC1 上定義的使用者主體名稱。  
  
-   [步驟 7:安裝和設定 RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a)。 安裝和設定 RSA、 RADIUS 和 OTP 伺服器，並設定 EDGE1 otp。  
  
-   [步驟 11:確認在 EDGE1 上的 OTP 狀況](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba)。 請確定 OTP 的狀態是狀況良好的遠端存取伺服器上。  
  
-   [步驟 8:測試從家用網路的子網路的 DirectAccess 連線](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14)。 測試從 NAT 裝置後方的 DirectAccess OTP 功能。  
  
-   [步驟 10:測試從網際網路的 DirectAccess 連線](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9)。 測試從網際網路的 DirectAccess 用戶端連線。  
  
-   [步驟 12:建立設定的快照](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4)。 完成測試實驗室之後，需要使用 DirectAccess OTP 設定的快照集，以便您可以返回至該更新版本，以測試其他狀況。  
  


