---
title: 步驟8設定 INET1
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 2e53eb8af7327932123fde8eb138611e5ae65242
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953674"
---
# <a name="step-8-configure-inet1"></a>步驟 8：設定 INET1

>適用於：Windows Server (半年度管道)、Windows Server 2016

若要讓用戶端電腦透過網際網路連線到遠端存取服務器，您必須在 INET1 上設定 2 EDGE1 的 DNS 專案。

### <a name="to-create-the-2-edge1-dns-entry"></a>建立2個 EDGE1 的 DNS 專案

1.  在 [**開始**] 畫面上，輸入**dnsmgmt.msc**，然後按 enter。

2.  在主控台樹中，開啟 [**正向對應區域**]，按一下 [ **contoso.com**]，然後以滑鼠右鍵按一下 [ **contoso.com**]，再按一下 [**新增主機] (A 或 AAAA) **。

3.  在 [**名稱**] 中，輸入**2-EDGE1**。 在 [ **IP 位址**] 中，輸入**131.107.0.20**。 依序按一下 [新增主機]****、[確定]****，然後按一下 [完成]****。



