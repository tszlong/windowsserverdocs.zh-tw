---
title: 使用 get AllServers 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 080836e406b329cf8c15f95ef6afc99973bb3e4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853089"
---
# <a name="using-the-get-allservers-command"></a>使用 get AllServers 命令



擷取所有的 Windows 部署服務伺服器的相關資訊。

> [!NOTE]
> 此命令可能需要長時間才能完成，如果您的環境中有多個 Windows 部署服務伺服器，或連結伺服器的網路連線速度很慢。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ 顯示: {組態 | 映像 | 所有}|指定要傳回資料的類型。</br>-   **設定**傳回伺服器組態資訊。</br>-   **映像**傳回伺服器上的映像群組、 開機映像和安裝映像相關資訊。</br>-   **所有**傳回伺服器組態和映像的資訊。|
|[/ 詳細]|當搭配 **/Show:Images**或是 **/Show:All**，傳回所有影像中每個映像的中繼資料。 如果 **/詳細**選項未指定，預設行為是傳回映像名稱、 描述和檔案名稱。|
|[/ 樹系: {是 | No}]|指定是否要傳回整個樹系或本機網域的相關資訊。 如果未指定此選項的值，預設行為是傳回本機網域中的伺服器。|

## <a name="BKMK_examples"></a>範例

若要檢視所有伺服器的相關資訊，請輸入：
```
WDSUTIL /Get-AllServers /Show:Config
```
若要檢視所有伺服器的詳細的資訊，請輸入：
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)