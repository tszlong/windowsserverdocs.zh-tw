---
title: 將必要的元資料標記新增至您的 Windows Server 相關文章
description: 您必須在 Windows Server 相關文章頂端新增為元資料標記的資訊清單。 必要的標籤可能會根據您的報告和小組需求而變更。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 7728d6e9bb2c1e5a6ad53f638f253c53a0720059
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717745"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>將必要的元資料標記新增至您的 Windows Server 相關文章

在每篇文章的頂端，都有特定的中繼資料，必須納入以供追蹤和 SEO 之用。 必要的標記可能會根據報告需求而變更。 但是，如果您需要新增/移除任何欄位，應該會收到通知。

它看起來應該像這樣，包括三個連字號 (---) 在上方和下方：

```markdown
---
title: Required. The title of the article should go here. This is used in SEO and search results.
description: Required. A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they're looking for.
author: Required. Your GitHub alias
ms.author: Required. Your Microsoft alias
manager: Optional. Your manager's Microsoft alias
ms.reviewer: Optional. The Microsoft alias for the primary PM for the feature/functionality
ms.topic: Type of article, including conceptual, how-to, hub-page, overview, quickstart, reference, sample, troubleshooting, or tutorial
ms.date: Date of change (MM/DD/YYYY)

---
```

## <a name="example"></a>範例

```markdown
---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
author: danielle-github
ms.author: danielle
manager: alainch
ms.reviewer: alainch
ms.topic: overview
ms.date: 07/06/2019

---
```