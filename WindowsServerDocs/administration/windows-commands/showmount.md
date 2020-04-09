---
title: showmount
description: Showmount –的 Windows 命令主題，它會顯示裝載的目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fa61d47bb14cf21d93beec0a6e9257b9f66737b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834221"
---
# <a name="showmount"></a>showmount

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用**showmount –** 來顯示裝載的目錄。  
  
## <a name="syntax"></a>語法  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>描述  
**Showmount –** 命令\-line 公用程式會在*伺服器*指定的電腦上，顯示 server for NFS 所匯出之掛接檔案系統的相關資訊。 如果未提供*伺服器*， **showmount –** 會顯示執行**showmount –** 命令之電腦的相關資訊。  
  
您必須提供下列其中一個選項：  
  
- **\-e** -顯示在伺服器上匯出的所有檔案系統。  
- **\-a** -顯示所有的網路檔案系統 \(NFS\) 用戶端，以及每個已掛接之伺服器上的目錄。  
- **\-d** -顯示伺服器上目前由 NFS 用戶端裝載的所有目錄。  
  
## <a name="see-also"></a>另請參閱  
[網路檔案系統服務命令參考資料](services-for-network-file-system-command-reference.md)  