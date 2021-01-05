---
title: 應在虛擬機器中啟用存放裝置控制器，以提供對連結儲存體的存取權
description: 瞭解當虛擬機器中有一或多個存放裝置控制器可停用時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
ms.date: 8/16/2016
ms.openlocfilehash: 4132eed0b376430922550d506613a496274b2f4c
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834833"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>應在虛擬機器中啟用存放裝置控制器，以提供對連結儲存體的存取權

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

*一或多個存放裝置控制器可能會在虛擬機器中停用。*

## <a name="impact"></a>影響

*虛擬機器無法使用與已停用的儲存控制器連線的儲存體。這會影響下列虛擬機器：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*使用客體作業系統中的裝置管理員來啟用所有存放裝置控制器。如果不需要儲存控制器，請使用 Hyper-v 管理員將它從虛擬機器中移除。*

如需有關如何使用裝置管理員的指示，請參閱客體作業系統中的說明。 如需有關如何移除存放裝置控制器的指示，請參閱下列程式。

#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>從虛擬機器移除 SCSI 存放裝置控制器

1.  開啟 Hyper-V 管理員。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。

2.  在結果窗格的 [ **虛擬機器**] 下，選取您要設定的虛擬機器。

3.  如果虛擬機器正在執行，請關閉虛擬機器。 在虛擬機器上按一下滑鼠右鍵，然後按一下 [ **關閉**]。

4.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。

5.  在 [ **設定** ] 對話方塊的左窗格中，按一下 [ **硬體**] 底下的 [ **SCSI 控制器**]。

6.  在右窗格中，按一下 [ **移除**]。



