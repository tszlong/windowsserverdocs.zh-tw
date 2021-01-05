---
title: 為虛擬機器提供所有可用的 integration services
description: 瞭解在虛擬機器上未啟用一或多個可用的整合服務時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
ms.date: 8/16/2016
ms.openlocfilehash: 0c618684e66f9a0af55e160ede8a263781d307a5
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833723"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>為虛擬機器提供所有可用的 integration services

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*虛擬機器上未啟用一或多個可用的整合服務。*

## <a name="impact"></a>影響

*下列虛擬機器將無法使用某些功能：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*如果這是故意的，則不需要採取進一步的動作。否則，請考慮在這些虛擬機器的設定中提供所有整合服務。*

某些 integration services 的可用性可透過虛擬機器設定來管理。

#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>管理 integration services 對虛擬機器的可用性

1.  開啟 Hyper-V 管理員。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。

2.  在結果窗格的 [ **虛擬機器**] 下，選取您要設定的虛擬機器。

3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。

4.  在 [ **管理**] 底下，按一下 [ **Integration Services**]。

5.  在整合服務清單中，選取您要提供給虛擬機器的每個服務的核取方塊。 如果核取方塊無法使用，則在虛擬機器中執行的客體作業系統不支援該特定的整合服務。



