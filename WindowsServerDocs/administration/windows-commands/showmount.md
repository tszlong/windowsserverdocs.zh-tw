---
title: showmount
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26acf24922e6ac53a5c902d65eb0f23bff0af93b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852349"
---
# <a name="showmount"></a>showmount

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用**showmount –** 顯示已掛接的目錄。  
  
## <a name="syntax"></a>語法  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>描述  
**Showmount**命令\-列公用程式顯示已掛接的檔案系統上所指定的電腦匯出 server for NFS 的相關資訊*Server*。 如果*伺服器*未提供，則**showmount**顯示所在的電腦的相關資訊**showmount –** 執行命令。  
  
您必須提供下列選項之一：  
  
- **\-e** -顯示所有檔案匯出伺服器上的系統。  
- **\-** -顯示所有的 Network File System \(NFS\)用戶端和每個已掛接的伺服器上的目錄。  
- **\-d** -顯示目前掛接 NFS 用戶端的伺服器上的所有目錄。  
  
## <a name="see-also"></a>另請參閱  
[如需網路檔案系統命令參考的服務](services-for-network-file-system-command-reference.md)  