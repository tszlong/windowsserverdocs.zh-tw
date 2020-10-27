---
title: Microsoft Server 啟用
description: 如何啟用 Windows Server 2016。
ms.date: 09/19/2018
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a45d5cc7a54
author: eross-msft
ms.author: thierryp
ms.localizationpriority: medium
ms.openlocfilehash: 232afbbb190227d25f7f1da2b3b9a105dbc3a312
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524943"
---
# <a name="windows-server-2016-activation"></a>Windows Server 2016 啟用

下面資訊概述最初規劃金鑰管理服務 (KMS) 啟用 (與 Windows Server 2016 相關) 時必須檢閱的考量。 如需與此處未列出舊版作業系統相關的 KMS 啟用資訊，請參閱[步驟 1：檢閱和選取啟用方法](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11))。

KMS 會使用用戶端-伺服器模式來啟用用戶端。 KMS 用戶端會連線到 KMS 伺服器 (稱為 KMS 主機) 來啟用。 KMS 主機必須在您的區域網路上。

KMS 主機不需要是專用伺服器，而且 KMS 可以與其他服務並存。 您可以在執行 Windows 10、Windows Server 2016、Windows Server 2012 R2、Windows 8.1 或 Windows Server 2012 的任何實體或虛擬系統上執行 KMS 主機。

在 Windows 10 或 Windows 8.1 上執行之 KMS 主機只能啟用執行用戶端作業系統的電腦。
下表針對包括 Windows Server 2016 和 Windows 10 用戶端的網路，摘要說明其 KMS 主機和用戶端需求。

> [!NOTE]
> KMS 伺服器上可能需要更新以支援這些新用戶端的啟動。 如果您收到啟用錯誤，請檢查您已進行此表格下方所列的適當更新。

|產品金鑰群組|KMS 可以裝載於|由這個 KMS 主機啟用的 Windows 版本|
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
|Windows Server 2016 的大量授權|Windows Server 2012<p>Windows Server 2012 R2<p>Windows Server 2016<p>|Windows Server 半年通道 <br><br>Windows Server 2016 (所有版本)<p>Windows 10 LTSB (2015 和 2016)<p>Windows 10 Professional<p>Windows 10 Enterprise<p>Windows 10 工作站專業版<br><br>Windows 10 Education<br><br>Windows Server 2012 R2 (所有版本)<p>Windows 8.1 專業版<p>Windows 8.1 Enterprise<p>Windows Server 2012 (所有版本)<p>Windows Server 2008 R2 (所有版本)<p>Windows Server 2008 (所有版本)<p>Windows 7 Professional<p>Windows 7 Enterprise|
|Windows 10 的大量授權|Windows 7<p>Windows 8.1<p> Windows 10|Windows 10 Professional<p> Windows 10 Professional N<p> Windows 10 Enterprise<p> Windows 10 Enterprise N<p> Windows 10 Education<p> Windows 10 Education N<p> Windows 10 企業版 LTSB (2015)<p> Windows 10 企業版 LTSB N (2015)<p> Windows 10 工作站專業版<br><br>Windows 8.1 專業版<p> Windows 8.1 Enterprise<p> Windows 7 Professional<p> Windows 7 Enterprise<p>|
|Windows Server 2012 R2 的大量授權，適用於 Windows 10|Windows Server 2008 R2<p> Windows Server 2012 Standard<p> Windows Server 2012 Datacenter<p> Windows Server 2012 R2 Standard<p>Windows Server 2012 R2 Datacenter|Windows 10 Professional<p> Windows 10 Enterprise<p>Windows 10 企業版 LTSB (2015)<br><br>Windows 10 工作站專業版<br><br>Windows 10 Education<br><br> Windows Server 2012 R2 (所有版本)<p> Windows 8.1 專業版<p> Windows 8.1 Enterprise<p> Windows Server 2012 (所有版本)<p> Windows Server 2008 R2 (所有版本)<p>Windows Server 2008 (所有版本)<p> Windows 7 Professional<p> Windows 7 Enterprise|

> [!NOTE]
> 根據 KMS 伺服器正在執行的作業系統，以及您要啟動的作業系統，您可能需要安裝一或多個更新：
> - Windows 7 或 Windows Server 2008 R2 上的 KMS 安裝必須更新，以支援執行 Windows 10 的用戶端啟用。 如需詳細資訊，請參閱[可讓 Windows 7 和 Windows Server 2008 R2 KMS 主機啟用 Windows 10 的更新](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10)。 
> - Windows Server 2012 上的 KMS 安裝必須更新，才能支援執行 Windows 10 和 Windows Server 2016 的用戶端、或更新版本之用戶端或伺服器作業系統啟用。 如需詳細資訊，請參閱 [Windows Server 2012 的 2016 年 7 月更新彙總套件](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012)。
> - Windows 8.1 或 Windows Server 2012 R2 上的 KMS 安裝必須更新，才能支援執行 Windows 10 和 Windows Server 2016 的用戶端、或更新版本之用戶端或伺服器作業系統啟用。 如需詳細資訊，請參閱 [Windows 8.1 和 Windows Server 2012 R2 的 2016 年 7 月更新彙總套件](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2)。 
> - Windows Server 2008 R2 無法更新以支援執行 Windows Server 2016 的用戶端、或更新版本之作業系統啟用。

單一 KMS 主機可以支援無數台 KMS 用戶端。 如果您有 50 台以上的用戶端，建議您至少配備兩個 KMS 主機，以免其中一個 KMS 主機突然無法使用。 大部分的組織最多只需要兩個 KMS 主機就可以運作整個基礎結構。

## <a name="addressing-kms-operational-requirements"></a>解決 KMS 運作需求
KMS 可以啟用實體和虛擬電腦，但是若要符合 KMS 啟用的條件，網路必須擁有至少最低數量的電腦 (稱為啟用門檻)。 只有在達到這個門檻之後，KMS 用戶端才能啟用。 為了確保達到啟用門檻，KMS 主機會計算網路上要求啟用的電腦數量。

KMS 主機會計算最近連線。 當用戶端或伺服器連絡 KMS 主機時，主機會將電腦識別碼加入其計數，然後在其回應中傳回目前的計數值。 如果計數夠高，會啟動用戶端或伺服器。 如果計數為 25 或更高，會啟動用戶端。 如果計數為 5 或更高，會啟用伺服器和 Microsoft Office 產品的大量版本。 KMS 只會計算過去 30 天的唯一連線，並只會儲存 50 個最近的連絡人。

KMS 啟用的有效期為 180 天，此期間稱為啟用有效間隔。 KMS 用戶端必須至少每 180 天連線到 KMS 主機更新啟用，才能保持啟用狀態。 根據預設，KMS 用戶端電腦會每 7 天嘗試更新它們的啟用。 用戶端啟用更新之後，啟用有效間隔會重新開始計算。

## <a name="addressing-kms-functional-requirements"></a>解決 KMS 功能需求

KMS 啟用需要 TCP/IP 連線。 KMS 主機和用戶端預設都是使用網域名稱系統 (DNS)。 根據預設，KMS 主機會使用 DNS 動態更新自動發佈 KMS 用戶端所需的資訊，讓 KMS 用戶端可以尋找並連線到 KMS 主機。 您可以接受這些預設設定，或者如果您有特殊的網路和安全性設定需求，可以手動設定 KMS 主機和用戶端。

啟用第一台 KMS 主機之後，第一台主機使用的 KMS 金鑰可以用來在網路上啟用最多 5 台額外的 KMS 主機。 KMS 主機啟用之後，系統管理員可以使用相同的金鑰重新啟用相同的主機最多 9 次。

如果組織需要 6 台以上的 KMS 主機，您應該為組織的 KMS 金鑰要求額外的啟用，例如，如果一個大量授權合約有 10 個實體位置，而您希望每個位置都有一個本機 KMS 主機。

> [!NOTE]
> 若要要求這種例外狀況，請連絡啟用客服中心。 如需詳細資訊，請參閱 [Microsoft 大量授權]( https://www.microsoft.com/licensing)。

根據預設，執行 Windows 10、Windows Server 2016、Windows 8.1、Windows Server 2012 R2、Windows Server 2012、Windows 7、Windows Server 2008 R2 大量授權版本的電腦為不需要另外設定的 KMS 用戶端。

如果您將電腦從 KMS 主機、MAK 或 Windows 零售版轉換成 KMS 用戶端，請安裝適用的 KMS 用戶端安裝識別碼。 如需詳細資訊，請參閱 [KMS 用戶端安裝識別碼](KMSclientkeys.md)。
