---
title: 啟用虛擬機器中的所有整合服務
description: 瞭解當一個或多個 integration services 停用或無法在虛擬機器中運作時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
ms.date: 8/16/2016
ms.openlocfilehash: 59ee108cbe2c6b3ff62c4cb6a9d57d1ec8c8ca65
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845927"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>啟用虛擬機器中的所有整合服務

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

*一或多個整合服務已停用或無法在虛擬機器中運作。*

## <a name="impact"></a>影響

*下列虛擬機器的服務或整合功能可能無法正確運作：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*使用 [服務] 嵌入式管理單元或 sc config 命令列工具，確認服務已設定為自動啟動，且未停止。*

#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>使用服務嵌入式管理單元來設定服務的啟動方式

1.  使用遠端桌面服務或虛擬機器連線連接到虛擬機器，並登入客體作業系統。

2.  開啟 [服務]。  (按一下 [ **開始**]，在 [ **開始搜尋** ] 方塊中按一下，輸入 **services.msc**，然後按 enter。 ) 

3.  在詳細資料窗格中，以滑鼠右鍵按一下您要設定的服務，然後按一下 [ **屬性**]。

4.  在 [ **一般** ] 索引標籤的 [ **啟動** 類型] 中，按一下 [ **自動**]。

#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>設定如何使用 SC 設定啟動服務

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [ **開始** ]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  以服務的名稱取代 <服務-名稱>，然後輸入：

    ```
    sc config <service-name> start=auto
    ```



