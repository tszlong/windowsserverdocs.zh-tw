---
title: bitsadmin setcredentials
description: 適用于 bitsadmin setcredentials 的 Windows 命令主題，其會將認證新增至作業。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 918bda93407e029cedaaf5eab937d1bb23dc3c4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849641"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

將認證新增至作業。

**BITS 1.2 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|目標|伺服器或 PROXY|
|會員|發生下列情形之一：</br>-基本-將使用者名稱和密碼以純文字傳送到伺服器或 proxy 的驗證配置。</br>-摘要式-一種挑戰回應驗證配置，其使用伺服器指定的資料字串來因應挑戰。</br>-NTLM —一種挑戰回應驗證配置，會使用使用者的認證在 Windows 網路環境中進行驗證。</br>-NEGOTIATE （也稱為簡單和受保護的協商通訊協定（Snego））是一種挑戰回應驗證配置，會與伺服器或 proxy 協調以判斷要使用哪種配置來進行驗證。 範例包括 Kerberos 通訊協定與 NTLM。</br>-PASSPORT-Microsoft 提供的集中式驗證服務，提供成員網站單一登入。|
|Username|提供的認證名稱|
|Password|與提供的使用者*名稱*相關聯的密碼|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會將認證新增至名為*myDownloadJob*的作業。
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)