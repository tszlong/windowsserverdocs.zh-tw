---
title: 儲存空間總覽
ms.prod: windows-server
ms.author: jgerend
manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 6785508704ff1eebcfd9b70a529ba9d615e5ce11
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858801"
---
# <a name="storage-spaces-overview"></a>儲存空間總覽

儲存空間是 Windows 和 Windows Server 中的一項技術，可協助保護您的資料免于磁片磁碟機失敗。 它在概念上類似于在軟體中實行的 RAID。 您可以使用儲存空間，將三個或多個磁片磁碟機組成一個存放集區，然後使用該集區的容量來建立儲存空間。 這些通常會儲存額外的資料複本，因此如果其中一部磁片磁碟機故障，您仍然會有資料的完整複本。 如果您的容量不足，只需要將更多磁片磁碟機新增到存放集區。

使用儲存空間有四種主要的方式：

- **在 WINDOWS 電腦上**-如需詳細資訊，請參閱[windows 10 中的儲存空間](https://windows.microsoft.com/windows-10/storage-spaces-windows-10)。
- 在**具有單一伺服器中所有存放裝置的獨立伺服器上**-如需詳細資訊，請參閱在[獨立伺服器上部署儲存空間](deploy-standalone-storage-spaces.md)。
- **在使用儲存空間直接存取並在每個叢集節點中使用本機的直接連接存放裝置的叢集伺服器上**-如需詳細資訊，請參閱[儲存空間直接存取總覽](storage-spaces-direct-overview.md)。
- **在具有一或多個共用 sas 儲存主機殼且保留所有磁片磁碟機的叢集伺服器上**-如需詳細資訊，請參閱[具有共用 SAS 的叢集上的儲存空間總覽](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11))。

