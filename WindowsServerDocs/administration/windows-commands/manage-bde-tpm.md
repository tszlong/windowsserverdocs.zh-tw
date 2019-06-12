---
title: 管理 bde tpm
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d41c846034ad421d0da81bda57acbcd419c1ae1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437396"
---
# <a name="manage-bde-tpm"></a>管理 bde: tpm

> 適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012
> 
> [!IMPORTANT]
> 此命令不支援在執行 Windows 8、 Windows Server 2012 或更新版本的作業系統的電腦上使用。 針對這些電腦，您可以使用[TPM 管理適用於 Windows PowerShell 的 cmdlet](https://docs.microsoft.com/en-us/powershell/module/trustedplatformmodule/)。
> 如果您使用此命令在執行 Windows 7 或 Windows Server 2008 的電腦上，您仍然可以設定電腦的受信任的平台模組 (TPM) 使用此命令。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。
> ## <a name="syntax"></a>語法
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> ### <a name="parameters"></a>參數
> 
> |    參數    |                                                                              描述                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -開啟     |              啟用和啟動 TPM，允許設定 TPM 擁有者密碼。 您也可以使用 **-t**為此命令縮寫版。              |
> | -takeownership  |                      需要 TPM 的擁有權，藉由設定擁有者密碼。 您也可以使用 **-o**為此命令縮寫版。                       |
> | <OwnerPassword> |                                                      表示您指定 tpm 的擁有者密碼。                                                       |
> |  -computername  | 指定該管理 bde.exe 會用來修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。 |
> |     <Name>      |    表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。     |
> |    -? 或 /？     |                                                               顯示簡短說明，在命令提示字元。                                                               |
> |   -help 或-h   |                                                             顯示完成的命令提示字元說明。                                                              |
> 
> ## <a name="BKMK_Examples"></a>範例
> 下列範例說明如何利用**tpm**命令來開啟 TPM。
> ```
> manage-bde  tpm -turnon
> ```
> 下列範例說明如何利用**tpm**命令來取得 TPM 的擁有權，並設定為擁有者密碼0wnerP@ss。
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>其他參考資料
> -   [命令列語法關鍵](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
