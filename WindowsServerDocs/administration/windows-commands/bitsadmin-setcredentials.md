---
title: bitsadmin setcredentials
description: 適用於 Windows 命令主題**bitsadmin setcredentials** -將認證新增至作業。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 923dcff7d268d40b72db3254e2a97c808c7c7253
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877389"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

將認證加入至作業。

**1.2 及更早版本的位元**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|目標|伺服器或 PROXY|
|配置|發生下列情形之一：</br>-基本 — 方式的使用者名稱和密碼傳送至伺服器或 proxy 的純文字的驗證配置。</br>-摘要-會針對該查問使用伺服器指定的資料字串的挑戰回應驗證配置。</br>-NTLM，使用 Windows 網路環境中驗證使用者的認證挑戰回應驗證配置。</br>-NEGOTIATE： 也稱為簡單和受保護的交涉通訊協定 (Snego) 是與伺服器或 proxy，以決定要用於驗證的配置會交涉的挑戰回應驗證配置。 範例包括 Kerberos 通訊協定與 NTLM。</br>-PASSPORT，提供成員網站單一登入的 Microsoft 所提供的集中式的驗證服務。|
|Username|提供的認證名稱|
|密碼|提供相關聯的密碼*使用者名稱*|

## <a name="BKMK_examples"></a>範例

下列範例 Adds 認證至名為作業*myDownloadJob*。
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)