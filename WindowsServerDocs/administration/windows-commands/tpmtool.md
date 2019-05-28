---
title: tpmtool
description: Windows 命令主題 tpmtool-取得受信任的平台模組的相關資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: d2939b693c3f9bd8d0d8c8e1fefdd5d8f892e4c9
ms.sourcegitcommit: 3582c38d87f23cc54467d63bf00c29ef07cdb7c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517191"
---
>[!IMPORTANT]
>預先發行的產品在正式發行前可能會大幅度修改，本文提供有關該產品的一些資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

# <a name="tpmtool"></a>tpmtool

此公用程式可用來取得相關資訊[信賴平台模組 (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview)。

如需如何使用此命令的範例，請參閱[範例](#tpmtool_examples)。

## <a name="syntax"></a>語法

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|getdeviceinformation|顯示的基本資訊的 TPM。|
|gatherlogs [輸出目錄路徑]|會收集 TPM 的記錄檔，並將它們放在指定的目錄。 根據預設，它們會放在目前的目錄。|
|/?|在命令提示字元顯示說明。|

## <a name="tpmtool_examples"></a>範例

若要顯示的 TPM 的基本資訊，請輸入：
```
tpmtool getdeviceinformation
```
若要收集 TPM 的記錄檔，並將它們放在目前目錄中，類型：
```
tpmtool gatherlogs
```
若要收集 TPM 的記錄檔，並置入`C:\Users\Public`，型別：
```
tpmtool gatherlogs C:\Users\Public
```
