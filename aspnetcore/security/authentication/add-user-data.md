---
title: 追加、ダウンロード、および ASP.NET Core プロジェクトの Id にカスタム ユーザー データの削除
author: rick-anderson
description: Id に、ASP.NET Core プロジェクトにカスタム ユーザー データを追加する方法を説明します。 GDPR ごとのデータを削除します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: 3ebb709cc40f6c2477ac57325d035b9b461e2eaf
ms.sourcegitcommit: 0d6f151e69c159d776ed0142773279e645edbc0a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2018
ms.locfileid: "35414995"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="5a235-104">追加、ダウンロード、および ASP.NET Core プロジェクトの Id にカスタム ユーザー データの削除</span><span class="sxs-lookup"><span data-stu-id="5a235-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="5a235-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a235-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a235-106">ここで説明する方法。</span><span class="sxs-lookup"><span data-stu-id="5a235-106">This article shows how to:</span></span>

* <span data-ttu-id="5a235-107">カスタム ユーザー データを ASP.NET Core web アプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="5a235-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="5a235-108">装飾できるでは、カスタム ユーザー データのモデル、 [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)属性のダウンロードと削除に自動的に使用可能になるようにします。</span><span class="sxs-lookup"><span data-stu-id="5a235-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="5a235-109">満たすデータがダウンロードされ、削除できないようにするのに役立ちます[GDPR](xref:security/gdpr)要件です。</span><span class="sxs-lookup"><span data-stu-id="5a235-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="5a235-110">プロジェクトのサンプルは、Razor ページの web アプリから作成でも、手順は ASP.NET Core MVC web アプリケーションに似ています。</span><span class="sxs-lookup"><span data-stu-id="5a235-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="5a235-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="5a235-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a235-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="5a235-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="5a235-113">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="5a235-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5a235-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a235-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5a235-115">Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5a235-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="5a235-116">プロジェクトに名前を**WebApp1**にする場合の名前空間と一致、[サンプルをダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)コード。</span><span class="sxs-lookup"><span data-stu-id="5a235-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="5a235-117">選択**ASP.NET Core Web アプリケーション** > **OK**</span><span class="sxs-lookup"><span data-stu-id="5a235-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="5a235-118">選択**ASP.NET Core 2.1**ドロップダウン リストに</span><span class="sxs-lookup"><span data-stu-id="5a235-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="5a235-119">選択**Web アプリケーション**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="5a235-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="5a235-120">プロジェクトをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="5a235-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5a235-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5a235-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="5a235-122">Identity scaffolder を実行します。</span><span class="sxs-lookup"><span data-stu-id="5a235-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5a235-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a235-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5a235-124">**ソリューション エクスプ ローラー**、プロジェクトを右クリックして >**追加** > **スキャフォールディングされた新しい項目**です。</span><span class="sxs-lookup"><span data-stu-id="5a235-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="5a235-125">左側のウィンドウから、**追加 Scaffold**ダイアログで、 **Identity** > **追加**です。</span><span class="sxs-lookup"><span data-stu-id="5a235-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="5a235-126">**追加 Identity**ダイアログ ボックスで、次のオプション。</span><span class="sxs-lookup"><span data-stu-id="5a235-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="5a235-127">既存のレイアウト ファイルを選択して *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5a235-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="5a235-128">オーバーライドする次のファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="5a235-128">Select the following files to override:</span></span>
    * <span data-ttu-id="5a235-129">**アカウントまたは登録**</span><span class="sxs-lookup"><span data-stu-id="5a235-129">**Account/Register**</span></span>
    * <span data-ttu-id="5a235-130">**アカウント//インデックスの管理**</span><span class="sxs-lookup"><span data-stu-id="5a235-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="5a235-131">選択、 **+** を新規作成するにはボタン**データ コンテキスト クラス**です。</span><span class="sxs-lookup"><span data-stu-id="5a235-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="5a235-132">種類を受諾 (**WebApp1.Models.WebApp1Context**プロジェクトの名前を付けた場合**WebApp1**)。</span><span class="sxs-lookup"><span data-stu-id="5a235-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="5a235-133">選択、 **+** を新規作成するにはボタン**ユーザー クラス**です。</span><span class="sxs-lookup"><span data-stu-id="5a235-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="5a235-134">種類を受諾 (**WebApp1User**プロジェクトの名前を付けた場合**WebApp1**) >**追加**です。</span><span class="sxs-lookup"><span data-stu-id="5a235-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="5a235-135">選択**追加**です。</span><span class="sxs-lookup"><span data-stu-id="5a235-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5a235-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5a235-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5a235-137">ASP.NET scaffolder を以前インストールしていない場合は、今すぐインストールします。</span><span class="sxs-lookup"><span data-stu-id="5a235-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="5a235-138">パッケージの参照を追加[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)プロジェクト (.csproj) ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="5a235-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="5a235-139">プロジェクト ディレクトリに、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a235-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="5a235-140">Identity scaffolder オプションの一覧を次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5a235-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="5a235-141">プロジェクト フォルダーでは、Identity scaffolder を実行します。</span><span class="sxs-lookup"><span data-stu-id="5a235-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="5a235-142">手順に従って[移行、UseAuthentication、およびレイアウト](xref:security/authentication/scaffold-identity#efm)を次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="5a235-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="5a235-143">移行を作成し、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="5a235-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="5a235-144">`UseAuthentication` に `Startup.Configure` を追加します。</span><span class="sxs-lookup"><span data-stu-id="5a235-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="5a235-145">追加`<partial name="_LoginPartial" />`レイアウト ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="5a235-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="5a235-146">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="5a235-146">Test the app:</span></span>
  * <span data-ttu-id="5a235-147">ユーザーを登録する</span><span class="sxs-lookup"><span data-stu-id="5a235-147">Register a user</span></span>
  * <span data-ttu-id="5a235-148">新しいユーザー名を選択 (横、**ログアウト**リンク)。</span><span class="sxs-lookup"><span data-stu-id="5a235-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="5a235-149">ウィンドウを拡大またはユーザー名およびその他のリンクを表示する、ナビゲーション バーのアイコンを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a235-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="5a235-150">選択、**個人データ**タブです。</span><span class="sxs-lookup"><span data-stu-id="5a235-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="5a235-151">選択、**ダウンロード**ボタンをクリックし、調査、 *PersonalData.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5a235-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="5a235-152">テスト、**削除**ボタンで、ユーザーのログオンを削除します。</span><span class="sxs-lookup"><span data-stu-id="5a235-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="5a235-153">Identity DB にカスタム ユーザー データを追加します。</span><span class="sxs-lookup"><span data-stu-id="5a235-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="5a235-154">更新プログラム、`IdentityUser`カスタム プロパティを持つクラスを派生します。</span><span class="sxs-lookup"><span data-stu-id="5a235-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="5a235-155">ファイルの名前は WebApp1 プロジェクトの名前を付けた場合*Areas/Identity/Data/WebApp1User.cs*です。</span><span class="sxs-lookup"><span data-stu-id="5a235-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="5a235-156">次のコード ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="5a235-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="5a235-157">修飾されたプロパティ、 [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)属性は。</span><span class="sxs-lookup"><span data-stu-id="5a235-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="5a235-158">削除されたときに、 *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor ページを呼び出す`UserManager.Delete`です。</span><span class="sxs-lookup"><span data-stu-id="5a235-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="5a235-159">によってダウンロードされたデータに含まれる、 *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor ページ。</span><span class="sxs-lookup"><span data-stu-id="5a235-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="5a235-160">Account/Manage/Index.cshtml ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="5a235-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="5a235-161">更新プログラム、`InputModel`で*Areas/Identity/Pages/Account/Manage/Index.cshtml.cs*次のようにコードを強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a235-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="5a235-162">更新プログラム、 *Areas/Identity/Pages/Account/Manage/Index.cshtml*次の強調表示されているマークアップ。</span><span class="sxs-lookup"><span data-stu-id="5a235-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="5a235-163">Account/Register.cshtml ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="5a235-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="5a235-164">更新プログラム、`InputModel`で*Areas/Identity/Pages/Account/Register.cshtml.cs*次のようにコードを強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a235-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="5a235-165">更新プログラム、 *Areas/Identity/Pages/Account/Register.cshtml*次の強調表示されているマークアップ。</span><span class="sxs-lookup"><span data-stu-id="5a235-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="5a235-166">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="5a235-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="5a235-167">カスタム ユーザー データの移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="5a235-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5a235-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a235-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5a235-169">Visual Studio で**Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="5a235-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5a235-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5a235-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="5a235-171">テストを作成、表示、ダウンロード、カスタム ユーザー データの削除</span><span class="sxs-lookup"><span data-stu-id="5a235-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="5a235-172">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="5a235-172">Test the app:</span></span>

* <span data-ttu-id="5a235-173">新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="5a235-173">Register a new user.</span></span>
* <span data-ttu-id="5a235-174">カスタム ユーザー データを表示、`/Identity/Account/Manage`ページ。</span><span class="sxs-lookup"><span data-stu-id="5a235-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="5a235-175">ダウンロードしてから、ユーザーの個人データを表示、`/Identity/Account/Manage/PersonalData`ページ。</span><span class="sxs-lookup"><span data-stu-id="5a235-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>