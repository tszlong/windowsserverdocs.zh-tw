---
title: Windows Admin Center SDK 案例研究-Thomas Krenn
description: Windows Admin Center SDK 案例研究-Thomas Krenn
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/24/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93b8a450aa86a454ec6febd349fcaa35df590266
ms.sourcegitcommit: 3be280c8638214857dc355b201eb56a04499a5e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67396758"
---
# <a name="thomas-krennag-extension"></a>Thomas Krenn.AG 延伸模組

## <a name="intuitive-server-and-storage-health-management"></a>直覺式的伺服器與存放裝置健康情況管理

Thomas Krenn.AG Windows Admin Center 擴充功能專為高可用性的 2 個節點[S2D Micro 叢集](https://www.thomas-krenn.com/en/products/application/software-defined-storage/s2d-micro-cluster.html)應用裝置。 使用者易記、 圖形化的 web 介面以視覺化方式呈現 Micro-叢集的健康情況狀態，透過簡單的儀表板，並可讓您向下切入的存放裝置、 網路介面或整個叢集，以檢視更多詳細資料。

擴充功能會提供直覺式的存取權通常所需的第一個層級服務及支援呼叫，例如序號、 軟體版本、 儲存體使用率等資訊。 它被設計為沒有先前的經驗，與 Windows Server 超交集基礎結構系統管理員很有用。

有幾個可用的深入解析：
- Micro 節點和 Micro 叢集的一般資訊
- OS / 開機裝置狀態
- 容量的 HDD 和 SSD 狀態的快取
- 叢集事件
- 網路狀態及資訊

您可以使用儀表板來判斷叢集的健康狀態和重要系統資訊，例如序號、 模型、 作業系統版本和使用率。 此外，儀表板也會顯示風扇、 NIC 和整體節點硬體健康狀態。

![Thomas Krenn 延伸模組](../../media/extend-case-study-thomas-krenn/thomas-krenn-1.png)

您可以鑽研到存放裝置，若要檢視序號、 智慧-狀態，以及容量使用率。 開機裝置也會顯示 wear 出指標，就重新磁區與電源配置時間，也就是 SSD 健全狀況的最佳指標。

![Thomas Krenn 延伸模組](../../media/extend-case-study-thomas-krenn/thomas-krenn-2.png)

叢集狀態圖示會展開以顯示叢集的作業詳細資料的摘要。

![Thomas Krenn 延伸模組](../../media/extend-case-study-thomas-krenn/thomas-krenn-3.png)

整個晚上為無法使用這個微-叢集的 Azure 雲端見證之後，則會一眼就足以找出問題。 立即按一下 [通知] 列出進行快速修復的相關事件。 叢集事件當地語系化，取決於基底的作業系統語言。 延伸模組本身支援英文和德文。

![Thomas Krenn 延伸模組](../../media/extend-case-study-thomas-krenn/thomas-krenn-4.png)

網路資訊也是隨手可得。

![Thomas Krenn 延伸模組](../../media/extend-case-study-thomas-krenn/thomas-krenn-5.png)

根據客戶意見反應，我們也已實作 「 暗色調模式 」 中 Windows Admin Center v1904 可用。 這是 soothing 深的資料中心，以及效能不佳反白顯示的封包和的儲藏室中。 它也能讓 Windows Admin Center 多藉由減少探照燈視力特定的系統管理員。

![Thomas Krenn 延伸模組](../../media/extend-case-study-thomas-krenn/thomas-krenn-6.png)

Thomas Krenn 立即實現可用性和協助工具，適用於定型的系統管理員，就是絕佳的客戶體驗到金鑰小型和中型企業市場的超交集基礎結構。 Thomas Krenn 的 Micro 叢集擴充功能完全補充 Windows Admin Center 的原生 HCI 管理功能，藉由包含儀表板上的專用硬體資訊並且重新群組中的新的重要的叢集健康情況資訊人類看得容易使用的介面。

在開發程序決定部署在高可用性組態叢集本身，確保即使在節點失敗之後的管理性上的 Windows Admin Center 1904年。 延伸模組會預先安裝，就如同整個作業系統。

Microsoft 正在開發的 Windows Admin Center 1904年與平行建置擴充功能。 密切合作及公開不斷的意見反應雙方共同已成功在 2019 年 4 月發行的產品之前解決的問題。 Thomas Krenn 是非常榮幸地是其中一種完全支援，以及實作 Windows Admin Center 1904 的新功能的第一個項目。
