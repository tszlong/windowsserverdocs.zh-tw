---
title: 使用磁碟保護保護系統磁碟區
description: 提供 MultiPoint 服務磁片保護的相關資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: 27bcce68cfe5856e987f5ad93460abd12217c26b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389514"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>使用磁碟保護保護系統磁碟區
MultiPoint 服務可讓您選擇在每次電腦啟動時立即清除系統磁碟區的任何變更。 如果您啟用磁片保護功能，則在下次電腦重新開機時，將會復原對磁片磁碟機的任何修改（例如設定損毀或引進惡意程式碼）。 對於想要確保每次都能載入已知「良好」或「黃金」軟體映射的系統管理員而言，這是方便的功能。 您可以排程自動更新或軟體修補，例如，在夜間的中間。 規劃考慮是指您是否要讓使用者能夠從網際網路進行變更，例如安裝軟體。 啟用這項功能後，如果您想要讓使用者能夠儲存檔案，檔案共用就必須位於系統磁碟區外。  
  
