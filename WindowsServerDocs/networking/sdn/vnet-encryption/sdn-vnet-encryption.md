---
title: 網路 virtual 加密
description: 本主題提供 Virtual 網路加密軟體定義網路功能在 Windows Server 的資訊
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 425cc1cff8c7241aeec7764b60c89b4586fd323b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-network-encryption"></a>網路 virtual 加密

>適用於：Windows Server

Virtual 網路加密提供 virtual 網路流量加密子網路標示為「加密支援」在彼此的虛擬電腦之間的能力。

這項功能會利用加密封包 virtual 子網路上資料流傳輸層級的安全性 (DTLS)。  DTLS 提供的防護功能竊取、竄改和冒名存取實體網路的任何人。

Virtual 網路加密 requries 在每個 SDN 上安裝的加密憑證支援 HYPER-V 主機時，在參考該憑證，並在每個 Virtual 網路設定的指紋 Network Controller 的認證物件這包含子網路需加密。

子網路上已支援加密之後, 子網路中的所有網路流量是會自動都加密。  這將會除了任何應用程式等級加密，也可能會才會生效。  會自動傳送加密資料傳輸與之間子網路，即使這兩個子網路標示為加密。  資料傳輸置於 virtual 網路邊界也會傳送加密。

適用於設定 Virtual 網路加密的詳細資訊，請查看[設定加密 Virtual 網路的](sdn-config-vnet-encryption.md)。

如果您必須要限制只有通訊加密子網路上的應用程式。  您可以使用存取控制清單 (Acl) 只允許通訊目前子網路中。  

適用於設定存取控制清單的詳細資訊，請查看[使用存取控制清單 (Acl) 來管理 Datacenter 網路流量 Flow](../manage/use-acls-for-traffic-flow.md)。