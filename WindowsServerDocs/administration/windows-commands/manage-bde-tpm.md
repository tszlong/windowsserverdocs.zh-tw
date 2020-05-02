---
title: manage-bde tpm
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb3859a1795959c90e71391b2926164165ef9ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724070"
---
# <a name="manage-bde-tpm"></a>manage-bde： tpm

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
> 
> [!IMPORTANT]
> 在執行 Windows 8、Windows Server 2012 或更新版本作業系統的電腦上，不支援使用此命令。 針對這些電腦，您可以使用[適用于 Windows PowerShell 的 TPM 管理 Cmdlet](https://docs.microsoft.com/powershell/module/trustedplatformmodule/)。
> 如果您在執行 Windows 7 或 Windows Server 2008 的電腦上使用此命令，您仍然可以使用此命令來設定電腦的信賴平臺模組（TPM）。
> ## <a name="syntax"></a>語法
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> #### <a name="parameters"></a>參數
> 
> |    參數    |                                                                              描述                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -homeautomation.turnon     |              啟用並啟動 TPM，以允許設定 TPM 擁有者密碼。 您也可以使用 **-t**做為此命令的縮寫版本。              |
> | -takeownership  |                      藉由設定擁有者密碼來取得 TPM 的擁有權。 您也可以使用 **-o**做為此命令的縮寫版本。                       |
> | <OwnerPassword> |                                                      代表您為 TPM 指定的擁有者密碼。                                                       |
> |  -computername  | 指定 manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
> |     <Name>      |    代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。     |
> |    -? 或/？     |                                                               在命令提示字元中顯示簡短說明。                                                               |
> |   -help 或-h   |                                                             在命令提示字元中顯示完整的說明。                                                              |
> 
> ## <a name="examples"></a>範例
> 說明如何使用 **-tpm**命令來開啟 tpm。
> ```
> manage-bde  tpm -turnon
> ```
> 說明如何使用**tpm**命令來取得 tpm 的擁有權，並將擁有者密碼設定0wnerP@ss為。
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>其他參考
> -   - [命令列語法關鍵](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
