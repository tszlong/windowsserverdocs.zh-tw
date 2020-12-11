---
description: 深入瞭解：儲存空間總覽
title: 儲存空間總覽
ms.author: jgerend
manager: dougkim
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: b2b18ac48f1e506adf1dc6bcd8217e9b68ec398b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046176"
---
# <a name="storage-spaces-overview"></a>儲存空間總覽

儲存空間是 Windows 和 Windows Server 中的一項技術，可協助保護您的資料免于磁片磁碟機失敗。 它在概念上類似于在軟體中執行的 RAID。 您可以使用儲存空間將三個或多個磁片磁碟機組成一個儲存集區，然後使用該集區中的容量來建立儲存空間。 這些通常會儲存資料的額外複本，因此如果您的其中一個磁片磁碟機失敗，您仍然會有資料的完整複本。 如果您的容量不足，只要新增更多磁片磁碟機到存放集區即可。

使用儲存空間有四種主要方式：

- **在 WINDOWS 電腦上** -如需詳細資訊，請參閱 [Windows 10 中的儲存空間](https://windows.microsoft.com/windows-10/storage-spaces-windows-10)。
- **在具有單一伺服器中所有存放裝置的獨立伺服器上** ：如需詳細資訊，請參閱在 [獨立伺服器上部署儲存空間](deploy-standalone-storage-spaces.md)。
- **在叢集伺服器上，使用儲存空間直接存取搭配本機、直接連接的存放裝置到每個叢集節點** -如需詳細資訊，請參閱 [儲存空間直接存取總覽](storage-spaces-direct-overview.md)。
- **在具有一或多個共用 sas 存放磁片櫃的叢集伺服器上** （如需詳細資訊），請參閱 [具有共用 SAS 的叢集上的儲存空間總覽](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11))。
