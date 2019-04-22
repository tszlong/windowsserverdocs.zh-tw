---
title: rpcinfo
description: 了解如何列出遠端電腦上的程式。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4aba1e57d5a61103310fbe7abcac391e543be5aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826369"
---
# <a name="rpcinfo"></a>rpcinfo

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

列出在遠端電腦上的程式。 **Rpcinfo**命令列公用程式可讓遠端程序呼叫 (RPC) 至 RPC 伺服器，並報告它所找到。 

## <a name="syntax"></a>語法
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/p [\<Node>]|列出所有註冊指定的主機上的連接埠對應程式的程式。 如果您未指定的節點 （電腦） 名稱，則程式會查詢本機主機上的連接埠對應程式。|
|b\<程式版本 >|從具有指定的程式和連接埠對應程式與已註冊的版本的所有網路節點中要求的回應。 您必須指定程式名稱或數字和版本號碼。|
|/t\<節點程式 > [\<版本 >]|您可以使用 TCP 傳輸通訊協定來呼叫指定的程式。 您必須指定一個節點 （電腦） 名稱和程式名稱。 如果您未指定版本，則程式會呼叫所有版本。|
|/u\<節點程式 > [\<版本 >]|您可以使用 UDP 傳輸通訊協定來呼叫指定的程式。 您必須指定一個節點 （電腦） 名稱和程式名稱。 如果您未指定版本，則程式會呼叫所有版本。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_Examples"></a>範例
若要列出所有已註冊的連接埠對應工具的程式，請輸入：
```
rpcinfo /p [<Node>]
```
若要從網路節點有指定的程式要求的回應，請輸入：
```
rpcinfo /b <Program version>
```
若要使用傳輸控制通訊協定 (TCP) 呼叫程式，請輸入：
```
rpcinfo /t <Node Program> [<version>]
```
您可以使用使用者資料包通訊協定 (UDP) 來呼叫程式：
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
