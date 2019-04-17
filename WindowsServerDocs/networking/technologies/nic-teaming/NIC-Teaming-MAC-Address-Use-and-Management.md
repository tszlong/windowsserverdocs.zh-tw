---
title: NIC 小組 MAC 位址使用與管理
description: 本主題提供如何 NIC 小組的使用媒體存取控制 (MAC) 位址 Windows Server 2016 中的相關資訊。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab624665c964b100e6b1ed633120299b55e37df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming-mac-address-use-and-management"></a>NIC 小組 MAC 位址使用與管理

>適用於：Windows Server 2016

當您切換獨立模式和地址 hash 或動態負載 distribution 設定 NIC 團隊時，小組使用媒體存取控制 (MAC) 位址輸出流量的主要 NIC 小組成員。 主要 NIC 小組成員是作業系統小組成員的初始設定中選取 [網路介面卡。  
  
主要的小組成員是您建立或重新啟動主機電腦之後，繫結至小組的第一個小組成員。 主要的小組成員可能會變更，每個開機不確定的方式，因為 NIC 停用日讓重要訊息或其他重新設定活動，主要小組成員可能會變更，以及小組的 MAC 位址可能有所不同。  
  
這不會通常會造成問題，但有可能會發生問題的一些案例。  
  
如果主要小組成員小組中移除，且再放入作業可能有的 MAC 位址衝突。 透過這個衝突身分查驗，停用以及然後小組介面。 停用，然後就小組介面的程序，會導致介面的剩餘的小組成員，藉此免除 MAC 位址衝突選取新的 MAC 位址。  
  
您可以設定 NIC 小組的 MAC 位址特定的 MAC 位址設定中的主要小組介面，就像您可以執行時設定的任何實體 NIC 的 MAC 位址  
  
## <a name="mac-address-use-on-transmitted-packets"></a>在傳送封包使用的 MAC 位址  
當您切換獨立模式和地址 hash 或動態負載 distribution 設定 NIC 團隊時，同時跨多個小組成員散發（例如單一 VM) 的單一來源的封包。 若要防止取得混淆參數，以避免 MAC 拍打鬧鐘來源的 MAC 位址會取代不同的 MAC 位址，在畫面上的主要小組成員以外的小組成員傳輸。 因此，每個小組成員使用不同的 MAC 位址，並 MAC 位址衝突，除非失敗之前，將無法。  
  
主要而偵測到失敗時，從開始 NIC 小組軟體小組成員，選擇要做為主要暫時的小組成員（亦即，做為主要小組成員開關切換至現在將會出現一個）上使用主要小組成員的 MAC 地址。  此變更僅適用於已主要小組成員主要小組成員的 MAC 地址做為來源的 MAC 位址移至 [傳送的資料傳輸。 其他資料傳輸持續會傳送具有 MAC 位址，其已經使用失敗之前任何來源。  
  
以下是清單描述 NIC 小組的 MAC 位址更換行為，根據小組的設定方式：  
  
1.  **地址 Hash distribution 的獨立切換模式**  
  
    -   所有 ARP 和奈秒封包都傳送主要小組成員  
  
    -   傳送 Nic 上主要小組成員以外的所有流量都傳送修改，以符合的而會都傳送來源 MAC 位址  
  
    -   所有主要小組成員上傳傳送與原始來源 MAC 地址（這可能是小組的來源的 MAC 位址）  
  
2.  **使用 HYPER-V 連接埠 distribution 獨立切換模式**  
  
    -   每個 vmSwitch 連接埠被 affinitized 小組成員  
  
    -   每個封包傳送的連接埠 affinitized 小組成員  
  
    -   不完成 MAC 取代任何來源  
  
3.  **動態通訊的獨立切換模式**  
  
    -   每個 vmSwitch 連接埠被 affinitized 小組成員  
  
    -   所有 ARP 日奈秒封包都傳送的連接埠 affinitized 小組成員  
  
    -   傳送 affinitized 的小組成員的小組成員封包已完成的 MAC 位址更換不來源  
  
    -   傳送 affinitized 的小組成員以外的小組成員封包會有來源 MAC 位址更換完成  
  
4.  **切換相關模式（所有散發）**  
  
    -   MAC 位址更換來源不被執行  
  
## <a name="see-also"></a>也了  
[建立新的 NIC 團隊主機電腦上 VM](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)  
[NIC 小組](NIC-Teaming.md)  
  


