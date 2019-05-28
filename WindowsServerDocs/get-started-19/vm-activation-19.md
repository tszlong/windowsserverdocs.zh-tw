---
title: 自動虛擬機器啟用
TOCTitle: Automatic VM Activation
description: 如何啟用 Windows Server 2019、 Windows Server 2016 和 Windows Server 2012 R2 中的 Vm
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
ms.openlocfilehash: 18e20433050371dc02782fb8630a885e53ae31ad
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63688699"
---
# <a name="automatic-virtual-machine-activation"></a>自動虛擬機器啟用

> 適用於：Windows Server 2019，Windows Server 半年通道，Windows Server 2016、windows Server 2012 R2

自動虛擬機器啟用 (AVMA) 的作用是購買證明機制，協助確保 Windows 產品的使用符合產品使用權及 Microsoft 軟體授權條款。

AVMA 可讓您在已正確啟用 Windows 的伺服器上安裝虛擬機器，不需要管理每部個別虛擬機器的產品金鑰，即使在沒有網路連線的環境中也是如此。 AVMA 會將虛擬機器啟用繫結至已授權的虛擬機器伺服器，並在它啟動時啟用虛擬機器。 AVMA 也會提供使用情況的即時報告，以及虛擬機器授權狀態的歷史資料。 在虛擬化伺服器上可取得報告與追蹤資料。

## <a name="practical-applications"></a>實際應用

AVMA 在使用大量授權或 OEM 授權啟用的虛擬化伺服器上提供數個好處。

伺服器資料中心管理員可使用 AVMA 執行下列工作：

  - 在遠端位置啟用虛擬機器

  - 在有/無網際網路連線的情況下啟用虛擬機器

  - 從虛擬化伺服器追蹤虛擬機器使用情況與授權，且不需要任何虛擬化系統的存取權

有沒有產品金鑰，來管理和讀取伺服器上的沒有貼紙。 即使是在虛擬化伺服器陣列之間移轉，虛擬機器仍會是啟用狀態，而且會持續運作。

服務提供商授權合約 (SPLA) 合作夥伴與其他主機提供者不需與租用戶共用產品金鑰，也不需存取租用戶的虛擬機器即可將它啟用。 使用 AVMA 時，虛擬機器啟用對租用戶來說是透明化的。 主機提供者可利用伺服器記錄來驗證授權相容，並追蹤用戶端使用歷程記錄。

## <a name="system-requirements"></a>系統需求

AVMA 需要執行 Windows Server 2019 Datacenter、 Windows Server 2016 Datacenter 或 Windows Server 2012 R2 的 Microsoft 虛擬化伺服器。 

以下是不同版本的主機可啟用來賓：

|伺服器主機版本|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

請注意，這些會啟用所有的版本 （資料中心、 標準或基本功能）。

這項工具不適用於其他虛擬化伺服器技術。

## <a name="how-to-implement-avma"></a>如何實作 AVMA

1.  在 Windows Server Datacenter 虛擬化伺服器上，安裝和設定 Microsoft HYPER-V 伺服器角色。 如需詳細資訊，請參閱 <<c0> [ 安裝 HYPER-V 伺服器](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md)。

2.  [建立虛擬機器](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md)和在其上安裝支援的伺服器作業系統。

3.  在虛擬機器中安裝 AVMA 金鑰。 在已提高權限的命令提示字元中執行下列命令：
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

虛擬機器會自動針對虛擬機器伺服器啟用授權。


> [!TIP]
> 您也可以在任何 Unattend.exe 安裝檔案中使用 AVMA 金鑰。


## <a name="avma-keys"></a>AVMA 金鑰

下列 AVMA 金鑰可用於 Windows Server 2019。

|版本|   AVMA 金鑰|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|
|Essentials|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
下列 AVMA 金鑰可用於 Windows Server 版 1809年。

|版本|   AVMA 金鑰|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|

下列 AVMA 金鑰可用於 Windows Server 版本 1803年和 1709年。

|版本|AVMA 金鑰|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


下列 AVMA 金鑰可用於 Windows Server 2016。

|版本|AVMA 金鑰|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Essentials|B4YNW-62DX9-W8V6M-82649-MHBKQ|


下列 AVMA 金鑰可用於 Windows Server 2012 R2。

|版本|AVMA 金鑰|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Essentials|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## <a name="reporting-and-tracking"></a>報告和追蹤

虛擬機器伺服器上的登錄 (KVP) 提供客體作業系統的即時追蹤資料。 因為登錄機碼會隨虛擬機器移動，所以您也可以取得授權資訊。 KVP 預設會傳回虛擬機器的相關資訊，包括下列各項：

  - 完整的網域名稱

  - 安裝的作業系統和 Service Pack

  - 處理器架構

  - IPv4 和 IPv6 網路位址

  - RDP 位址

如需如何取得這項資訊的詳細資訊，請參閱[HYPER-V 指令碼：KVP guestintrinsicexchangeitems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx)。


> [!NOTE]
> KVP 資料並不安全。 不但可以修改，系統也不會監看是否變更。



> [!IMPORTANT]
> 如果使用其他產品金鑰 (零售、OEM 或大量授權金鑰) 取代 AVMA 金鑰，就應該移除 KVP 資料。


有關 AVMA 要求的歷史資料可在虛擬機器伺服器的記錄檔中找到 (EventID 12310)。

因為 AVMA 啟用程序是通透的，所以不會顯示錯誤訊息。 不過，虛擬機器上的記錄檔會擷取下列事件 (EventID 12309)。

|通知|描述|
|-|-|
|AVMA 成功|虛擬機器已啟用。|
|無效的主機|虛擬機器伺服器無回應。 當伺服器不是執行支援的 Windows 版本，就可能發生這種情形。|
|無效的資料|這通常是虛擬機器伺服器與虛擬機器之間的通訊失敗所導致，常見的原因是損毀、加密或資料不符。|
|啟用被拒|虛擬機器伺服器無法啟用客體作業系統，因為 AVMA 識別碼不相符。|

