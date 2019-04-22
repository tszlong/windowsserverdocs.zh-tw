---
title: bitsadmin create
description: 適用於 Windows 命令主題**bitsadmin 建立**-指定的顯示名稱建立一個傳送工作。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6ce5a4fdc21d879bf0a265e3c4185d83311464a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817189"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

指定的顯示名稱，建立一個傳送工作。 從伺服器的作業傳輸資料下載到本機檔案中。 上傳作業傳輸資料，從本機檔案至伺服器。 上傳-回覆作業將資料從本機檔案傳輸至伺服器，並從伺服器接收回覆檔案。

使用[bitsadmin 繼續](bitsadmin-resume.md)切換至啟動的傳送佇列中的作業。

## <a name="syntax"></a>語法

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|型別|-   **/ 下載**將資料從伺服器傳送到本機檔案。<br />-   **/ 上傳**將資料從本機檔案傳輸到伺服器。<br />-   **/ 上傳-回覆**將資料從本機檔案傳輸到伺服器，並從伺服器接收回覆檔案。<br />-此參數的預設值 **/下載**時未指定命令列上。|
|DisplayName|顯示名稱指派給新建立的作業。|

**1.2 及更早版本的位元**: /Upload 和 /Upload-Reply 類型無法使用。

## <a name="BKMK_examples"></a>範例

建立名為的下載作業*myDownloadJob*。

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
