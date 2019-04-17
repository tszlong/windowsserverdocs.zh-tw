---
title: 啟用自動虛擬機器
TOCTitle: Automatic VM Activation
description: 如何啟用 Windows Server 2019、 Windows Server 2016 和 Windows Server 2012 R2 中的虛擬機器
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 62873140c8e114ba537dc4fd3ff7c44868c33243
ms.sourcegitcommit: ca5c80bdb903b282e292488172a7cc92546574c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "4375485"
---
# 啟用自動虛擬機器

> 適用於： Windows Server 2019，Windows Server 半年通道，Windows Server 2016、 Windows Server 2012 R2

自動虛擬機器啟用 (AVMA) 做為證明的機制，協助確保 Windows 產品可依據 Microsoft 軟體授權條款與產品使用權限。

AVMA 可讓您的正確啟動的 Windows 伺服器上安裝的虛擬機器，而不需管理產品金鑰，針對每個個別的虛擬機器，即使在中斷連線的環境。 AVMA 將虛擬機器啟用繫結到授權的虛擬化伺服器，並啟動虛擬機器，它啟動時。 AVMA 也會提供即時報告上使用狀況和虛擬機器的授權狀態的歷程記錄資料。 報告和追蹤資料是適用於虛擬化 server。

## 實際應用

虛擬化在伺服器上所啟動的使用大量授權或 OEM 授權，AVMA 提供幾個優點。

伺服器的資料中心管理員可以使用 AVMA 來執行下列動作：

  - 啟用遠端位置中的虛擬機器

  - 啟動虛擬機器使用或不使用網際網路連線

  - 從虛擬化伺服器，追蹤虛擬機器使用狀況和授權，而不需要任何虛擬化的系統上的存取權限

有任何產品金鑰來管理和在伺服器上的沒有貼紙讀取。 在虛擬機器已啟用，且仍會繼續執行，即使它跨陣列的虛擬化伺服器移轉。

服務提供者授權合約 (SPLA) 合作夥伴和其他主機服務提供者不需要租用戶與分享產品金鑰，或是會存取租用戶的虛擬機器，以啟動它。 使用 AVMA 時，虛擬機器啟用是透明的租用戶。 主機服務提供者可以使用伺服器記錄檔，以確認相容性，授權和追蹤用戶端使用歷程記錄。

## 系統需求

AVMA 需要 Microsoft 虛擬化伺服器執行 Windows Server 2019 Datacenter、 Windows Server 2016 Datacenter 或 Windows Server 2012 R2。 

以下是不同版本的主機可以啟用 「 來賓：

|伺服器主機版本|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

請注意這些啟用所有版本 （Datacenter、 Standard、 或 Essentials）。

此工具不使用其他伺服器虛擬化技術。

## 如何實作 AVMA

1.  在 Windows Server Datacenter 虛擬化伺服器上，安裝，並設定 Microsoft HYPER-V 伺服器角色。 如需詳細資訊，請參閱[安裝 HYPER-V 伺服器](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md)。

2.  [建立虛擬機器](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md)並安裝支援的伺服器作業系統，在其上。

3.  在虛擬機器中安裝 AVMA 金鑰。 從提升權限的命令提示字元中，執行下列命令：
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

在虛擬機器將會自動啟用對虛擬化伺服器授權。


> [!TIP]
> 您也可以採用 AVMA 中的索引鍵的任何 Unattend.exe 安裝程式檔案。


## AVMA 金鑰

下列 AVMA 機碼可用於 Windows Server 2019。

|版本|   AVMA 金鑰|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2 個 D 623 4GF74|
|基本資訊|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
下列 AVMA 機碼可用於 Windows Server 版本 1809年。

|版本|   AVMA 金鑰|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2 個 D 623 4GF74|

下列 AVMA 機碼可用於 Windows Server 版本 1803年及 1709年。

|版本|AVMA 金鑰|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


下列 AVMA 機碼可用於 Windows Server 2016。

|版本|AVMA 金鑰|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|基本資訊|B4YNW-62DX9-W8V6M-82649-MHBKQ|


下列 AVMA 機碼可用於 Windows Server 2012 R2。

|版本|AVMA 金鑰|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|基本資訊|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## 報告和追蹤

虛擬化伺服器上的登錄 (KVP) 提供客體作業系統的即時追蹤資料。 因為登錄機碼移與虛擬機器時，您可以取得也授權資訊。 根據預設 KVP 會傳回虛擬機器，包括下列資訊：

  - 完整的網域名稱

  - 作業系統和安裝服務套件

  - 處理器架構

  - IPv4 與 IPv6 網路位址

  - RDP 位址

如需有關如何取得這項資訊的詳細資訊，請參閱[HYPER-V 指令碼： 看著 KVP GuestIntrinsicExchangeItems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx)。


> [!NOTE]
> KVP 資料不會受到保護。 它可修改且不受監視的變更。



> [!IMPORTANT]
> 如果 AVMA 鍵已取代為另一個產品金鑰 （零售、 OEM 或大量授權金鑰），應該會移除 KVP 資料。


歷史有關 AVMA 要求都有提供資料虛擬化伺服器 （事件識別碼 12310） 上的記錄檔。

因為 AVMA 啟用程序是透明的所以不會顯示錯誤訊息。 不過，在虛擬機器 （事件識別碼 12309） 上的記錄檔中擷取的下列事件。

|通知|描述|
|-|-|
|AVMA 成功|啟動虛擬機器。|
|無效的主機|虛擬化伺服器沒有回應。 當伺服器不在執行支援的 Windows 版本，這可能會發生。|
|無效的資料|這通常會從失敗中虛擬化伺服器和虛擬機器，通常是因損毀、 加密或資料集不符之間的通訊。|
|啟用拒絕|虛擬化伺服器無法啟動，客體作業系統，因為不符合 AVMA 識別碼。|

