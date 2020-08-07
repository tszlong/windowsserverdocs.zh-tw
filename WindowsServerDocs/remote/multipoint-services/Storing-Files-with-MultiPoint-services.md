---
title: 使用 MultiPoint 服務儲存檔案
description: 瞭解 MultiPoint 服務中的檔案儲存體
ms.date: 07/22/2016
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6991a62fde0e0083eb5544eed6ec49fef2b10569
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951623"
---
# <a name="storing-files-with-multipoint-services"></a>使用 MultiPoint 服務儲存檔案
MultiPoint 服務支援以下列方式儲存使用者檔案：

-   **在硬碟機的作業系統磁碟分割上。** 根據預設，MultiPoint 服務會將使用者檔案儲存在作業系統的硬碟上。

-   **在硬碟的個別磁碟分割上。** 當您第一次設定 MultiPoint 服務系統時，您可以*分割*硬碟。 也就是說，您可以設定磁片磁碟機的某個區段，讓它的功能如同另一個磁片磁碟機一樣。 這可讓您更輕鬆地還原或升級作業系統，而不會影響使用者檔案。 如需詳細資訊，請參閱 Windows Server 技術文件庫中的[建立磁碟分割或邏輯磁碟機](https://go.microsoft.com/fwlink/?LinkId=182618)。

-   **在額外的內部或外部硬碟上。** 您可以將額外的內部或外部硬碟連接到 MultiPoint 服務，以儲存和備份資料。

-   **在共用網路資料夾中。** 若要讓使用者檔案可從任何工作站取得，您可以在網路上建立共用資料夾。 這需要另一部電腦或伺服器，以及執行 MultiPoint 服務的電腦。 如果有可用的檔案伺服器，這是儲存檔案的建議方法。

    針對執行不含檔案伺服器2-3 之 MultiPoint 服務的小型系統，其中一部 MultiPoint 服務電腦可作為所有 MultiPoint 服務電腦的檔案伺服器。 接著，您會針對 MultiPoint 服務上作為檔案伺服器的所有使用者建立使用者帳戶。

