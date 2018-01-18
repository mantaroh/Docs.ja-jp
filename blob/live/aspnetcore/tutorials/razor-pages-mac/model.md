---
title: "Visual Studio for Mac を使用する Razor ページ アプリへのモデルの追加"
author: rick-anderson
description: "Visual Studio for Mac を使用する ASP.NET Core での Razor ページ アプリへのモデルの追加"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC,モデル"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 648ecd3a782fa489b727982ce5f7a2087539bf38
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="f6a94-104">Visual Studio for Mac を使用する ASP.NET Core での Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="f6a94-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f6a94-105">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="f6a94-105">Add a data model</span></span>

* <span data-ttu-id="f6a94-106">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-106">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="f6a94-107">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="f6a94-107">Name the folder *Models*.</span></span>
* <span data-ttu-id="f6a94-108">*Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-108">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="f6a94-109">**[新しいファイル]** ダイアログで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-109">In the **New File** dialog:</span></span>

  * <span data-ttu-id="f6a94-110">左側のウィンドウで **[全般]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-110">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="f6a94-111">中央ウィンドウで **[空のクラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-111">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="f6a94-112">クラスに **Movie** という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-112">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="f6a94-113">たとえば、`services.AddDbContext<MovieContext>(options =>` 行の `MovieContext` など、赤の波線を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="f6a94-113">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="f6a94-114">**[Quick Fix]\(クイック フィックス\)、[using RazorPagesMovie.Models;](\RazorPagesMovie.Models; を使用\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-114">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="f6a94-115">Visual studio によって using ステートメントが追加されます。</span><span class="sxs-lookup"><span data-stu-id="f6a94-115">Visual studio adds the using statement.</span></span>

<span data-ttu-id="f6a94-116">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-116">Build the project to verify you don't have any errors.</span></span>

![[作成] ページ](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="f6a94-118">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="f6a94-118">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="f6a94-119">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="f6a94-119">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="f6a94-120">このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="f6a94-121">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="f6a94-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="f6a94-122">*.csproj* ファイルを編集するには、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="f6a94-123">**[ファイル]**、**[開く]** を選択して、*.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="f6a94-124">**[オプション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-124">Select **Options**.</span></span>
* <span data-ttu-id="f6a94-125">**[Open with](\次で開く\)** を **[ソース コード エディター]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-125">Change **Open with** to **Source Code Editor**.</span></span>

![csproj ファイルの編集](model/csproj.png)

<span data-ttu-id="f6a94-127">`Microsoft.EntityFrameworkCore.Tools.DotNet` ツール参照の 2 つ目の **\<ItemGroup >** への追加</span><span class="sxs-lookup"><span data-stu-id="f6a94-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="f6a94-128">プロジェクトへの Pages/Movies ファイルの追加</span><span class="sxs-lookup"><span data-stu-id="f6a94-128">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="f6a94-129">Visual Studio で *Pages* フォルダーを右クリックし、**[追加]、[既存のフォルダーの追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-129">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="f6a94-130">*Movies* フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-130">Select the *Movies* folder.</span></span>
* <span data-ttu-id="f6a94-131">*[Chosse files to include in the project](\プロジェクトに含めるファイルを選択する\)* ダイアログで、**[Include All](\すべて含める\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-131">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="f6a94-132">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f6a94-132">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f6a94-133">[前: はじめに](xref:tutorials/razor-pages-mac/razor-pages-start)
[次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f6a94-133">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>