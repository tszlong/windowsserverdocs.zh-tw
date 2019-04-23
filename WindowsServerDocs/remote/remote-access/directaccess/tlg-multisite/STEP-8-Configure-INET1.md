---
title: 步驟 8 設定 INET1
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 707591604a1d030b3abba9395081d2c2e4b7fb1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838809"
---
# <a name="step-8-configure-inet1"></a>步驟 8:設定 INET1

>適用於：Windows Server （半年通道），Windows Server 2016

若要啟用用戶端電腦透過網際網路連接到遠端存取伺服器，您必須設定 DNS 項目 2 EDGE1 上 INET1。  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>若要建立的 2 EDGE1 DNS 項目  
  
1.  在 **開始**畫面上，輸入**dnsmgmt.msc**，然後按 ENTER 鍵。  
  
2.  在主控台樹狀目錄中，開啟**正向對應區域**，按一下**contoso.com**，然後以滑鼠右鍵按一下**contoso.com**，然後按一下 **新增主機 （A 或 AAAA）**.  
  
3.  在 **名稱**，型別**2 EDGE1**。 在  **IP 位址**，型別**131.107.0.20**。 依序按一下 [新增主機]、[確定]，然後按一下 [完成]。  
  


