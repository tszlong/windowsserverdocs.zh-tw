---
title: 開始使用軟體清查記錄
description: 描述如何安裝和開始使用軟體清查記錄
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6944eac179c605db6c7b6f3e08f87c2329fb777f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435371"
---
# <a name="get-started-with-software-inventory-logging"></a>開始使用軟體清查記錄

>適用於：Windows Server （半年通道）、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

 軟體清查記錄收集每個伺服器上的 Microsoft 軟體清查資料。 使用軟體清查記錄之前與 Windows Server 2012 R2，請確定 Windows Update [KB 3000850](https://support.microsoft.com/kb/3000850)並[KB 3060681](https://support.microsoft.com/kb/3060681)安裝在將清查的每個系統上。 Windows 不不需要任何更新適用於 Windows Server 2016。 此外，如果您想要使用 SIL 的功能，將資料轉送至彙總伺服器，您必須具備有效的 SSL 憑證為您的網路。

## <a name="BKMK_OVER"></a>功能描述
Windows Server 中的軟體清查記錄功能有一組簡單的 PowerShell Cmdlet，可協助伺服器系統管理員擷取伺服器上安裝的 Microsoft 軟體清單。 它也提供功能，定期收集並透過網路，使用 HTTPS 通訊協定將此資料轉送到目標 Web 伺服器，以便進行彙總。 管理功能 (主要是每小時的收集和轉送) 也是使用 PowerShell 命令完成。

> [!NOTE]
> 執行 web 服務的彙總伺服器可以分開設定。 深入了解 [軟體清查記錄彙總工具](software-inventory-logging-aggregator.md)。

> [!IMPORTANT]
> 此功能不會將軟體清查記錄收集的資料傳送給 Microsoft。

## <a name="BKMK_APP"></a>實際的應用程式
軟體清查記錄的目的是為了擷取部署在伺服器本機上的 Microsoft 軟體正確資料時，能夠減少運作成本，尤其是在 IT 環境中的許多伺服器上 (假設已在 IT 環境中部署並啟用)。 將此資料轉送到彙總伺服器 (如果由 IT 系統管理員分開設定) 的功能，可以統一且自動化的程序從一個位置收集資料。 雖然這個工作也能透過直接查詢介面來完成，但是軟體清查記錄能使用由每部伺服器啟動的轉送 (經由網路) 架構，這樣可以克服一般會在許多軟體清查和資產管理案例中遇到的機器探索難題。 SSL 是用來保護透過 HTTPS 轉送到系統管理員彙總伺服器的資料。 將資料放在一個位置 (在一部伺服器上) 可以更輕鬆地分析、操控及報告資料。 請特別注意，此功能不會將此資料傳送給 Microsoft。 軟體清查記錄資料和功能僅供伺服器軟體授權擁有者及系統管理員使用。

軟體清查記錄可以協助伺服器系統管理員執行下列工作：

-   從 Windows Server 以遠端方式，依需求來擷取軟體和伺服器清查資訊。

-   透過啟用每部伺服器的軟體清查記錄功能，並選擇 Web 伺服器目標 URI 和驗證用的憑證指紋，彙總各種軟體資產管理案例的軟體和伺服器清查資訊。

## <a name="see-also"></a>另請參閱
[軟體清查記錄彙總工具](https://technet.microsoft.com/library/mt572043.aspx)<br>
[管理軟體清查記錄](manage-software-inventory-logging.md)<br>
[在 Windows PowerShell 中的軟體清查記錄 Cmdlet](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Microsoft Assessment and Planning Toolkit](https://www.microsoft.com/download/en/details.aspx?id=7826)
[Volume Activation Management Tool](http://blogs.technet.com/b/volume-licensing/)

