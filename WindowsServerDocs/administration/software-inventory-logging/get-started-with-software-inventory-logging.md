---
title: 使用軟體清查記錄開始
description: 說明如何安裝和開始使用軟體清查記錄
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: brentf
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 76a8f0c55a604fd3f17963ec18df31ed6faf340a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628278"
---
# <a name="get-started-with-software-inventory-logging"></a>使用軟體清查記錄開始

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 軟體清查記錄會以每一伺服器為基礎收集 Microsoft 軟體清查資料。 在搭配 Windows Server 2012 R2 使用軟體清查記錄之前，請確定 Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850) 和 [KB 3060681](https://support.microsoft.com/kb/3060681) 都安裝在將清查的每個系統上。 Windows Server 2016 不需要 Windows Update。 此外，如果您想要使用 SIL 的功能將資料轉送到匯總伺服器，請確定您的網路有有效的 SSL 憑證。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
Windows Server 中的軟體清查記錄功能有一組簡單的 PowerShell Cmdlet，可協助伺服器系統管理員擷取伺服器上安裝的 Microsoft 軟體清單。 它也提供功能，定期收集並透過網路，使用 HTTPS 通訊協定將此資料轉送到目標 Web 伺服器，以便進行彙總。 管理功能 (主要是每小時的收集和轉送) 也是使用 PowerShell 命令完成。

> [!NOTE]
> 執行 web 服務的彙總伺服器可以分開設定。 深入了解[軟體清查記錄彙總工具](software-inventory-logging-aggregator.md)。

> [!IMPORTANT]
> 軟體清查記錄所收集的資料都不會在功能的功能中傳送給 Microsoft。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
軟體清查記錄的目的是為了擷取部署在伺服器本機上的 Microsoft 軟體正確資料時，能夠減少運作成本，尤其是在 IT 環境中的許多伺服器上 (假設已在 IT 環境中部署並啟用)。 將此資料轉送到彙總伺服器 (如果由 IT 系統管理員分開設定) 的功能，可以統一且自動化的程序從一個位置收集資料。 雖然這個工作也能透過直接查詢介面來完成，但是軟體清查記錄能使用由每部伺服器啟動的轉送 (經由網路) 架構，這樣可以克服一般會在許多軟體清查和資產管理案例中遇到的機器探索難題。 SSL 是用來保護透過 HTTPS 轉送到系統管理員匯總伺服器的資料。 將資料放在一個位置 (在一部伺服器上) 可以更輕鬆地分析、操控及報告資料。 請特別注意，此功能不會將此資料傳送給 Microsoft。 軟體清查記錄資料和功能僅供伺服器軟體的授權擁有者和系統管理員使用。

軟體清查記錄可以協助伺服器系統管理員執行下列工作：

-   從 Windows Server 以遠端方式，依需求來擷取軟體和伺服器清查資訊。

-   藉由啟用每部伺服器的軟體清查記錄功能，並選擇網頁伺服器目標 URI 和憑證指紋來進行驗證，以匯總各種軟體資產管理案例的軟體和伺服器清查資訊。

## <a name="see-also"></a>另請參閱
[軟體清查記錄彙總工具](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/mt572043(v=ws.11))<br>
[管理軟體清查記錄](manage-software-inventory-logging.md)<br>
[Windows PowerShell 中的軟體清查記錄 Cmdlet](/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)<br>
[Microsoft 評估和規劃工具](https://www.microsoft.com/download/en/details.aspx?id=7826) 
 組[大量啟用管理工具](https://blogs.technet.com/b/volume-licensing/)