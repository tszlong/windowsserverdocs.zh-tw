---
title: bitsadmin removecredentials
description: 適用於 Windows 命令主題**bitsadmin removecredentials** -從作業中移除認證。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cbd65442ff0d74ec1179a49df5d4a94785f3dd25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822289"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

從作業中移除認證。

**1.2 及更早版本的位元**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /RemoveCredentials <Job> <Target> <Scheme>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|目標|伺服器或 PROXY|
|配置|發生下列情形之一：</br>-基本 — 方式的使用者名稱和密碼傳送至伺服器或 proxy 的純文字的驗證配置。</br>-摘要-會針對該查問使用伺服器指定的資料字串的挑戰回應驗證配置。</br>-NTLM，使用 Windows 網路環境中驗證使用者的認證挑戰回應驗證配置。</br>-NEGOTIATE： 也稱為簡單和受保護的交涉通訊協定 (Snego) 是與伺服器或 proxy，以決定要用於驗證的配置會交涉的挑戰回應驗證配置。 範例包括 Kerberos 通訊協定與 NTLM。</br>-PASSPORT，提供成員網站單一登入的 Microsoft 所提供的集中式的驗證服務。|

## <a name="BKMK_examples"></a>範例

下列範例會移除名為作業中的認證*myDownloadJob*。
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)