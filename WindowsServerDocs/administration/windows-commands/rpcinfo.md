---
title: rpcinfo
description: 瞭解如何列出遠端電腦上的程式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4212951a5aee46be893069c15dba4b210aca253d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722277"
---
# <a name="rpcinfo"></a>rpcinfo

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出遠端電腦上的程式。 **Rpcinfo**命令列公用程式會對 RPC 伺服器進行遠端程序呼叫（rpc），並報告它找到的內容。 

## <a name="syntax"></a>語法
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/p [\<Node>]|列出所有使用指定主機上的通訊埠對應程式註冊的程式。 如果您未指定節點（電腦）名稱，程式會查詢本機主機上的埠對應程式。|
|/b \<程式版本>|要求所有網路節點的回應，其中已向通訊埠對應工具註冊指定的程式和版本。 您必須同時指定程式名稱或號碼和版本號碼。|
|/t \<Node 程式> [\<version>]|會使用 TCP 傳輸通訊協定來呼叫指定的程式。 您必須同時指定節點（電腦）名稱和程式名稱。 如果您未指定版本，則程式會呼叫所有版本。|
|/u \<Node 程式> [\<version>]|會使用 UDP 傳輸通訊協定來呼叫指定的程式。 您必須同時指定節點（電腦）名稱和程式名稱。 如果您未指定版本，則程式會呼叫所有版本。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例
若要列出所有向通訊埠對應程式註冊的程式，請輸入：
```
rpcinfo /p [<Node>]
```
若要從具有指定程式的網路節點要求回應，請輸入：
```
rpcinfo /b <Program version>
```
若要使用「傳輸控制通訊協定」（TCP）來呼叫程式，請輸入：
```
rpcinfo /t <Node Program> [<version>]
```
使用使用者資料包協定（UDP）來呼叫程式：
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>其他參考資料
-   - [命令列語法關鍵](command-line-syntax-key.md)
