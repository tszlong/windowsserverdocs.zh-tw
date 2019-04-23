---
title: 支援的 Windows 客體作業系統的 Windows Server 上的 HYPER-V
description: 列出支援作為虛擬機器中的來賓 Windows 作業系統。 也提供類似的文章對於舊版的 HYPER-V 連結。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: 5f0e91f3202f09d340154b49408c56752a9de577
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874209"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>支援的 Windows 客體作業系統的 Windows Server 上的 HYPER-V

>適用於：Windows Server 2016、windows Server 2019

HYPER-V 支援數個版本的虛擬機器，做為客體作業系統中執行的 Windows Server、 Windows，以及 Linux 散發套件。 本文章涵蓋支援的 Windows Server 和 Windows 客體作業系統。 針對 Linux 和 FreeBSD 的散發套件，請參閱[支援的 Linux 和 FreeBSD 虛擬機器，在 Windows 上的 hyper-v](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。  
    
某些作業系統有內建的整合服務。 有些則需要您安裝或升級整合服務做為個別的步驟，設定虛擬機器中的作業系統之後。 如需詳細資訊，請參閱下列各節並[Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services)。  
  
## <a name="supported-windows-server-guest-operating-systems"></a>支援的 Windows Server 客體作業系統  

以下是在 Windows Server 2016 和 Windows Server 2019 的 hyper-v 支援做為客體作業系統版本的 Windows Server。 
  
|客體作業系統 (伺服器)|虛擬處理器數量的上限|整合服務|附註|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows Server 2019 |層代 2，針對 240<br>第 1 代 64|內建|| 
|Windows Server 2016 |層代 2，針對 240<br>第 1 代 64|內建|| 
|Windows Server 2012 R2 |64|內建||  
|Windows Server 2012 |64|內建||  
|Windows Server 2008 R2 含 Service Pack 1 (SP 1)|64|在設定好來賓作業系統之後，請安裝所有重大的 Windows 更新。|Datercenter、Enterprise、Standard 以及 Web 版本。|
|Windows Server 2008 Service Pack 2 (SP2)|8|在設定好來賓作業系統之後，請安裝所有重大的 Windows 更新。|Datacenter、Enterprise、Standard 以及 Web 版本 (32 位元與 64 位元)。|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>支援的 Windows 用戶端客體作業系統  

以下是 Windows 用戶端 hyper-v 在 Windows Server 2016 和 Windows Server 2019 做為客體作業系統所支援的版本。
  
|客體作業系統 (用戶端)|虛擬處理器數量的上限|整合服務|附註|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|內建||  
|Windows 8.1|32|內建||  
|Windows 7 含 Service Pack 1 (SP 1)|4|在設定好來賓作業系統之後，請升級整合服務。|旗艦版、企業版和專業版版本 (32 位元與 64 位元)。|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>在其他版本的 Windows 上的客體作業系統支援  

下表提供其他版本的 Windows 上的 HYPER-V 支援的客體作業系統的相關資訊的連結。  
  
|主機作業系統|主題|  
|-------------------------|---------|  
|Windows 10|[用戶端 HYPER-V 的 Windows 10 中支援的客體作業系統](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 與 Windows 8.1|-   [支援的 Windows 的 Windows Server 2012 R2 和 Windows 8.1 中 HYPER-V 客體作業系統](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Linux 和 FreeBSD 虛擬機器之 HYPER-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 與 Windows 8|[支援的 Windows 的 Windows Server 2012 和 Windows 8 中之 HYPER-V 客體作業系統](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 和 Windows Server 2008 R2|[關於虛擬機器和客體作業系統](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Microsoft 為客體作業系統所提供的支援  

Microsoft 透過下列方式提供客體作業系統的支援：  
  
-   Microsoft 作業系統和整合服務中發現的問題可獲得 Microsoft 支援服務的支援。  
  
-   經過作業系統廠商認證可在 Hyper-V 執行的其他作業系統上發現的問題，由廠商提供支援。  
  
-   若為在其他作業系統中發現的問題，Microsoft 會將問題提交給 [TSANet](https://www.tsanet.org/)，其為多個廠商支援的社群。  
  
## <a name="see-also"></a>另請參閱  
  
-   [Linux 和 FreeBSD 虛擬機器之 HYPER-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [用戶端 HYPER-V 的 Windows 10 中支援的客體作業系統](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



