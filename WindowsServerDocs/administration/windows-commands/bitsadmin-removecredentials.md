---
title: bitsadmin removecredentials
description: '**Bitsadmin removecredentials**的 Windows 命令主題-從作業移除認證。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34a5bae9304a9db9f47f437276270ca06b1ebeee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380819"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

從作業中移除認證。

**BITS 1.2 和更早版本**： 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /RemoveCredentials <Job> <Target> <Scheme>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|目標|伺服器或 PROXY|
|會員|發生下列情形之一：</br>-基本-將使用者名稱和密碼以純文字傳送到伺服器或 proxy 的驗證配置。</br>-摘要式-一種挑戰回應驗證配置，其使用伺服器指定的資料字串來因應挑戰。</br>-NTLM —一種挑戰回應驗證配置，會使用使用者的認證在 Windows 網路環境中進行驗證。</br>-NEGOTIATE （也稱為簡單和受保護的協商通訊協定（Snego））是一種挑戰回應驗證配置，會與伺服器或 proxy 協調以判斷要使用哪種配置來進行驗證。 範例包括 Kerberos 通訊協定與 NTLM。</br>-PASSPORT-Microsoft 提供的集中式驗證服務，提供成員網站單一登入。|

## <a name="BKMK_examples"></a>典型

下列範例會從名為*myDownloadJob*的作業中移除認證。
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)