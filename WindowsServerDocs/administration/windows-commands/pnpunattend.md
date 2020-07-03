---
title: pnpunattend
description: Pnpunattend 命令的參考文章，它會審核計算機上的設備磁碟機，以及執行無提示驅動程式安裝。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: cb01e2afa763d3e2c906d1b3ac5f194143caf114
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924248"
---
# <a name="pnpunattend"></a>pnpunattend

會針對設備磁碟機審核電腦，並執行自動驅動程式安裝，或在不安裝和的情況下搜尋驅動程式，並選擇性地將結果報告到命令列。 使用此命令來指定特定硬體裝置的特定驅動程式安裝。

## <a name="prerequisites"></a>必要條件

較舊版本的 Windows 作業系統需要初步準備。 使用此命令之前，您必須完成下列工作：

1. 為您要安裝的驅動程式建立目錄。 例如，在**C:\Drivers\Video**上建立用於視訊卡驅動程式的資料夾。

2. 下載並解壓縮您裝置的驅動程式套件。 將包含您作業系統版本之 INF 檔案的子資料夾內容，以及您所建立之 [video] 資料夾的任何子資料夾複製到其中。 例如，將視頻驅動程式檔案複製到**C:\Drivers\Video**。

3. 將系統內容路徑變數新增至您在步驟1中建立的資料夾。例如， **C:\Drivers\Video**。

4. 建立下列登錄機碼，然後針對您建立的**DriverPaths**金鑰，將**值資料**設定為**1**。

5. 若為 Windows®7，請流覽登錄路徑： **HKEY_LOCAL_Machine \\ \software\microsoft\windows server\ NT\CurrentVersion**，然後建立金鑰： **UnattendSettings\PnPUnattend\DriverPaths \\ **

## <a name="syntax"></a>語法

```
PnPUnattend.exe auditsystem [/help] [/?] [/h] [/s] [/l]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| auditsystem | 指定線上驅動程式安裝。<p>必要項，但使用 **/help**或 **/？** 執行此命令時除外 參數。 |
| /s | 選擇性。 指定不安裝就搜尋驅動程式。 |
| /l | 選擇性。 指定在命令提示字元中顯示此命令的記錄資訊。 |
| `/? | /help` | 選擇性。 在命令提示字元中顯示此命令的說明。 |

### <a name="examples"></a>範例

[到] 命令會顯示如何使用**PNPUnattend.exe**來審核電腦是否有可能的驅動程式更新，然後將結果回報給命令提示字元，請輸入：

```
pnpunattend auditsystem /s /l
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
