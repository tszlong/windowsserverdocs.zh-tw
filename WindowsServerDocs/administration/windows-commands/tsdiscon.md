---
title: tsdiscon
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1b5fca329864ebed9eab66671a17493f0fc3ca8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440908"
---
# <a name="tsdiscon"></a>tsdiscon

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

工作階段中斷連線的遠端桌面工作階段主機 (rd 工作階段主機) 伺服器。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|\<SessionId>|指定要中斷連線的工作階段識別碼。|
|\<SessionName>|指定要中斷連線的工作階段的名稱。|
|/server:\<伺服器名稱 >|指定包含您想要中斷連線工作階段的終端機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。|
|/v|顯示已執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您必須擁有完全控制權限，或中斷連線中斷工作階段中的另一位使用者的特殊存取權限。
-   如果指定任何工作階段識別碼或工作階段名稱，則**tsdiscon**中斷目前工作階段連線。
-   當您重新連線到該工作階段，而不遺失資料，會自動執行時中斷連線工作階段正在執行的任何應用程式。 使用**重設工作階段**結束執行中應用程式的中斷連線工作階段，但請注意，這可能會導致資料遺失的工作階段。
-   **/Server**只有當您使用時，才是必要參數**tsdiscon**從遠端伺服器。
-   無法中斷連線的主控台工作階段。

## <a name="BKMK_examples"></a>範例
- 若要中斷連線目前工作階段，請輸入：
  ```
  tsdiscon
  ```
- 若要中斷連線工作階段 10，請輸入：
  ```
  tsdiscon 10
  ```
- 若要中斷連線工作階段 term04，請輸入：
  ```
  tsdiscon TERM04
  ```
  #### <a name="additional-references"></a>其他參考資料
  [命令列語法重點](command-line-syntax-key.md)
  [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
