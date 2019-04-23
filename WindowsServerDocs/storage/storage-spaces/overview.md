---
title: 儲存空間概觀
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 9977bb35be3676e31cdcab7322b5b5a2cfc67609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832909"
---
# <a name="storage-spaces-overview"></a>儲存空間概觀

儲存空間是一種技術在 Windows 和 Windows Server，可協助保護您的資料從磁碟機失敗。 就概念上類似於 RAID，在軟體中實作。 您可以使用儲存空間分組彙整到存放集區的三個或多個磁碟機，並接著使用從該集區容量建立儲存空間。 這些物件通常會儲存額外的資料複本，如果其中一個磁碟機失敗，您仍必須保持不變的資料複本。 如果您執行容量不足，只要新增更多磁碟機的存放集區。

有四種主要的方式，使用儲存空間：

- **在 Windows PC** -如需詳細資訊，請參閱 <<c2> [ 在 Windows 10 中的儲存空間](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10)。
- **在單一伺服器的所有儲存體的獨立伺服器上**-如需詳細資訊，請參閱 <<c2> [ 獨立伺服器上部署儲存空間](deploy-standalone-storage-spaces.md)。
- **在每個叢集節點的本機、 直接連結儲存體使用儲存空間直接存取叢集伺服器上**-如需詳細資訊，請參閱 <<c2> [ 儲存空間直接存取概觀](storage-spaces-direct-overview.md)。
- **具有一或多個共用 SAS 儲存體機箱持有的所有磁碟機的叢集伺服器上**-如需詳細資訊，請參閱 <<c2> [ 具有共用 SAS 概觀的叢集上的儲存空間](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11))。

