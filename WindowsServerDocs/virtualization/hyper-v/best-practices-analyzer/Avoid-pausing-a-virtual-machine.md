---
title: 避免暫停虛擬機器
description: 瞭解當這部伺服器有一部或多部虛擬機器處於暫停狀態時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
ms.date: 8/16/2016
ms.openlocfilehash: c1f5ba73a181ef0ec89dfa83bdbb82b3b49e9c6d
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834613"
---
# <a name="avoid-pausing-a-virtual-machine"></a>避免暫停虛擬機器

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*此伺服器有一或多部虛擬機器處於暫停狀態。*

## <a name="impact"></a>影響

*根據可用的記憶體數量，您可能無法執行額外的虛擬機器。*

暫停的虛擬機器不會釋出其配置的記憶體，這表示記憶體無法用來啟動其他虛擬機器。

## <a name="resolution"></a>解決方案

*如果這是故意的，則不需要採取進一步的動作。否則，請考慮繼續這些虛擬機器或將其關閉。*

#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>使用 Hyper-v 管理員恢復虛擬機器

1.  開啟 Hyper-V 管理員。  (從伺服器管理員的 [ **工具** ] 功能表中，按一下 [ **hyper-v 管理員**]。 ) 

2.  從 [ **虛擬機器** ] 清單中，尋找狀態為 [已 **暫停**] 的虛擬機器。

    > [!IMPORTANT]
    > 當虛擬機器的實體儲存體上沒有剩餘的可用空間時，就會發生已 **暫停** 的狀態。 在您嘗試繼續處於此狀態的虛擬機器之前，請釋放實體儲存體上的可用空間。

3.  在每個虛擬機器名稱上按一下滑鼠右鍵，然後按一下 [ **繼續**]。 此舉會讓虛擬機器返回執行狀態。 之後，如果您想要關閉虛擬機器，請再次以滑鼠右鍵按一下該虛擬機器，然後選擇 [ **關機**]。

#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>使用 Windows PowerShell 恢復虛擬機器

當您取得主機上的所有虛擬機器之後，您可以使用篩選和管線在單一命令中完成此動作。 輸入：

```
get-vm | where state -eq 'paused' | resume-vm
```



