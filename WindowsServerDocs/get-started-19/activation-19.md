---
title: Windows Server 2019 Activation
description: 如何啟用 Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 4cc669fee4fbd31edc8813f16761ecb9f90532df
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810913"
---
# <a name="windows-server-2019-activation"></a>Windows Server 2019 Activation

>適用於：Windows Server 2019，Windows Server 2016

下面資訊概述最初的規劃考量，您必須檢閱牽涉到 Windows Server 2019 的金鑰管理服務 (KMS) 啟用的。 牽涉到比此處所列舊作業系統的 KMS 啟用的相關資訊，請參閱[步驟 1:檢閱和選取啟用方法](https://technet.microsoft.com/library/jj134256(WS.11).aspx)。

KMS 會使用用戶端-伺服器模式來啟用用戶端。 KMS 用戶端會連線到 KMS 伺服器 (稱為 KMS 主機) 來啟用。 KMS 主機必須在您的區域網路上。

KMS 主機不需要是專用伺服器，而且 KMS 可以與其他服務並存。 您可以在執行 Windows 10，Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows 8.1 或 Windows Server 2012 的任何實體或虛擬系統上執行 KMS 主機。

在 Windows 10 或 Windows 8.1 上執行的 KMS 主機只能啟用執行用戶端作業系統的電腦。
下表摘要說明網路包含 Windows Server 2016、 Windows Server 2019 和 Windows 10 用戶端的 KMS 主機和用戶端需求。

> [!NOTE]
> - KMS 伺服器上可能需要更新以支援這些新用戶端的啟動。 如果您收到啟用錯誤，請檢查您已進行此表格下方所列的適當更新。
> - 如果您正在使用虛擬機器，請參閱[自動虛擬機器啟用](vm-activation-19.md)資訊和 AVMA 金鑰。

|產品金鑰群組|KMS 可以裝載於|由這個 KMS 主機啟用的 Windows 版本|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|適用於 Windows Server 2019 的大量授權|Windows Server 2012 R2<br /><br />Windows Server 2016<br /><br />Windows Server 2019<br /><br />|Windows Server 半年通道<br /><br />Windows Server 2019 （所有版本）<br /><br />Windows Server 2016 (所有版本)<br /><br />Windows 10 企業版 LTSC 2019 <br /><br />Windows 10 企業版 LTSC N 2019<br /><br />Windows 10 LTSB (2015 和 2016)<br /><br />Windows 10 專業版<br /><br />Windows 10 企業版<br /><br />Windows 10 工作站專業版<br /><br />Windows 10 教育版<br /><br />Windows Server 2012 R2 (所有版本)<br /><br />Windows 8.1 專業版<br /><br />Windows 8.1 Enterprise<br /><br />Windows Server 2012 (所有版本)<br /><br />Windows Server 2008 R2 （所有版本）<br /><br />Windows Server 2008 （所有版本）<br /><br />Windows 7 專業版<br /><br />Windows 7 企業版<br />| 
|Windows Server 2016 的大量授權|Windows Server 2012<br /><br />Windows Server 2012 R2<br /><br />Windows Server 2016<br /><br />|Windows Server 半年通道 <br><br>Windows Server 2016 (所有版本)<br /><br />Windows 10 LTSB (2015 和 2016)<br /><br />Windows 10 專業版<br /><br />Windows 10 企業版<br /><br />Windows 10 工作站專業版<br><br>Windows 10 教育版<br><br>Windows Server 2012 R2 (所有版本)<br /><br />Windows 8.1 專業版<br /><br />Windows 8.1 Enterprise<br /><br />Windows Server 2012 (所有版本)<br /><br />Windows Server 2008 R2 （所有版本）<br /><br />Windows Server 2008 （所有版本）<br /><br />Windows 7 專業版<br /><br />Windows 7 企業版<br /><br />| 
|Windows 10 的大量授權|Windows 7<br /><br /> Windows 8.1<br /><br /> Windows 10|Windows 10 專業版<br /><br /> Windows 10 Professional N<br /><br /> Windows 10 企業版<br /><br /> Windows 10 Enterprise N<br /><br /> Windows 10 教育版<br /><br /> Windows 10 Education N<br /><br /> Windows 10 企業版 LTSB (2015)<br /><br /> Windows 10 企業版 LTSB N (2015)<br /><br /> Windows 10 工作站專業版<br><br>Windows 8.1 專業版<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows 7 專業版<br /><br /> Windows 7 企業版<br /><br />|  
|「適用於 Windows 10 的 Windows Server 2012 R2」的大量授權|Windows Server 2008 R2<br /><br /> Windows Server 2012 Standard<br /><br /> Windows Server 2012 Datacenter<br /><br /> Windows Server 2012 R2 Standard<br /><br />Windows Server 2012 R2 Datacenter|Windows 10 專業版<br /><br /> Windows 10 企業版<br /><br />Windows 10 企業版 LTSB (2015)<br><br>Windows 10 工作站專業版<br><br>Windows 10 教育版<br><br> Windows Server 2012 R2 (所有版本)<br /><br /> Windows 8.1 專業版<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows Server 2012 (所有版本)<br /><br /> Windows Server 2008 R2 （所有版本）<br /><br /> Windows Server 2008 （所有版本）<br /><br />Windows 7 專業版<br /><br /> Windows 7 企業版|

> [!NOTE]  
> 根據 KMS 伺服器正在執行的作業系統，以及您要啟動的作業系統，您可能需要安裝一或多個更新：
> - Windows 7 或 Windows Server 2008 R2 上的 KMS 安裝必須更新，以支援執行 Windows 10 的用戶端啟用。 如需詳細資訊，請參閱 <<c0>  [ ，可讓 Windows 7 和 Windows Server 2008 R2 KMS 主機啟用 Windows 10 更新](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10)。  
> - Windows Server 2012 上的 KMS 安裝必須更新，才能支援執行 Windows 10 和 Windows Server 2016 或 Windows Server 2019，用戶端或更新版本的用戶端或伺服器作業系統的啟動。 如需詳細資訊，請參閱 <<c0>  [ 2016 年 7 月更新彙總套件，適用於 Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012)。 
> - Windows 8.1 或 Windows Server 2012 R2 上的 KMS 安裝必須更新，才能支援執行 Windows 10 和 Windows Server 2016 或 Windows Server 2019，用戶端或更新版本的用戶端或伺服器作業系統的啟動。 如需詳細資訊，請參閱 <<c0>  [ 2016 年 7 月的 Windows 8.1 和 Windows Server 2012 R2 更新彙總套件](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2)。  
> - 無法更新 Windows Server 2008 R2，以支援執行 Windows Server 2016、 Windows Server 2019 或更新版本之作業系統的用戶端啟動。 

單一 KMS 主機可以支援無數台 KMS 用戶端。 如果您有 50 台以上的用戶端，建議您至少配備兩個 KMS 主機，以免其中一個 KMS 主機突然無法使用。 大部分的組織最多只需要兩個 KMS 主機就可以運作整個基礎結構。

# <a name="addressing-kms-operational-requirements"></a>解決 KMS 運作需求
KMS 可以啟用實體和虛擬電腦，但是若要符合 KMS 啟用的條件，網路必須擁有至少最低數量的電腦 (稱為啟用門檻)。 只有在達到這個門檻之後，KMS 用戶端才能啟用。 為了確保達到啟用門檻，KMS 主機會計算網路上要求啟用的電腦數量。

KMS 主機會計算最近連線。 當用戶端或伺服器連絡 KMS 主機時，主機會將電腦識別碼加入其計數，然後在其回應中傳回目前的計數值。 如果計數夠高，會啟動用戶端或伺服器。 如果計數為 25 或更高，會啟動用戶端。 如果計數為 5 或更高，會啟用伺服器和 Microsoft Office 產品的大量版本。 KMS 只會計算過去 30 天的唯一連線，並只會儲存 50 個最近的連絡人。

KMS 啟用的有效期為 180 天，此期間稱為啟用有效間隔。 KMS 用戶端必須至少每 180 天連線到 KMS 主機更新啟用，才能保持啟用狀態。 根據預設，KMS 用戶端電腦會每 7 天嘗試更新它們的啟用。 用戶端啟用更新之後，啟用有效間隔會重新開始計算。

# <a name="addressing-kms-functional-requirements"></a>解決 KMS 功能需求

KMS 啟用需要 TCP/IP 連線。 KMS 主機和用戶端預設都是使用網域名稱系統 (DNS)。 根據預設，KMS 主機會使用 DNS 動態更新自動發佈 KMS 用戶端所需的資訊，讓 KMS 用戶端可以尋找並連線到 KMS 主機。 您可以接受這些預設設定，或者如果您有特殊的網路和安全性設定需求，可以手動設定 KMS 主機和用戶端。

啟用第一台 KMS 主機之後，第一台主機使用的 KMS 金鑰可以用來在網路上啟用最多 5 台額外的 KMS 主機。 KMS 主機啟用之後，系統管理員可以使用相同的金鑰重新啟用相同的主機最多 9 次。

如果組織需要 6 台以上的 KMS 主機，您應該為組織的 KMS 金鑰要求額外的啟用，例如，如果一個大量授權合約有 10 個實體位置，而您希望每個位置都有一個本機 KMS 主機。

> [!NOTE] 
> 若要要求這種例外狀況，請連絡啟用客服中心。 如需詳細資訊，請參閱 [Microsoft 大量授權](https://go.microsoft.com/fwlink/?LinkID=73076)。

會執行大量授權版本的 Windows 10、 Windows Server 2019、 Windows Server 2016、 Windows 8.1、 Windows Server 2012 R2、 Windows Server 2012、 Windows 7、 Windows Server 2008 R2 的電腦，根據預設，KMS 用戶端不需要額外的設定所需。

如果您將電腦從 KMS 主機、MAK 或 Windows 零售版轉換成 KMS 用戶端，請安裝適用的 KMS 用戶端安裝識別碼。 如需詳細資訊，請參閱 <<c0>  [ KMS Client Setup Keys](../get-started/KMSclientkeys.md)。 
 

 
