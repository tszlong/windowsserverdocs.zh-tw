---
title: Sc delete
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05b276de04d4250cc03e4b2976bf8c1330ef82ce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835381"
---
# <a name="sc-delete"></a>Sc delete



從登錄中刪除服務子機碼。 如果服務正在執行，或有其他進程具有服務的開啟控制碼，服務就會標示為要刪除。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
sc [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName >|指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例（UNC）格式（例如，\\\\myserver）。 若要在本機執行 SC.EXE，請省略此參數。|
|\<ServiceName >|指定**getkeyname**作業所傳回的服務名稱。|
|?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

使用 [**控制台**] 上的 [**新增或移除程式**] 來刪除 DHCP、DNS 或任何其他內建的作業系統服務。 請注意，[**新增或移除程式**] 不只會移除服務的登錄子機碼，也會卸載服務並刪除其任何快捷方式。

## <a name="examples"></a>範例

若要從本機電腦上的登錄中刪除服務子機碼**NewServ** ，請輸入：
```
sc delete newserv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)