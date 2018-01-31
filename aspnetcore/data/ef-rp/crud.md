---
title: "Razor ページの EF コア CRUD - 2 8"
author: rick-anderson
description: "作成、読み取り、更新、EF コアで削除する方法を示します"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="64524-103">作成、読み取り、更新、および削除 - EF コア Razor ページ (2/8)</span><span class="sxs-lookup"><span data-stu-id="64524-103">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="64524-104">によって[Tom Dykstra](https://github.com/tdykstra)、 [Jon P Smith](https://twitter.com/thereformedprog)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64524-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="64524-105">このチュートリアルでは、スキャフォールディング CRUD (作成、読み取り、更新、削除) コードの確認し、カスタマイズができます。</span><span class="sxs-lookup"><span data-stu-id="64524-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="64524-106">注: EF コアに重点を置いてこれらのチュートリアルの変更に複雑さを軽減するには、EF コア コードは、モデルで使用される、Razor ページ ページ。</span><span class="sxs-lookup"><span data-stu-id="64524-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="64524-107">一部の開発者で、サービス レイヤー、またはリポジトリ パターンを使用して、UI (Razor ページ) とデータ アクセス層の間で抽象化レイヤーを作成します。</span><span class="sxs-lookup"><span data-stu-id="64524-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="64524-108">このチュートリアル、作成、編集、削除、および詳細 Razor ページで、*学生*フォルダーを変更します。</span><span class="sxs-lookup"><span data-stu-id="64524-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="64524-109">スキャフォールディングのコードでは、作成、編集、および削除のページの次のパターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="64524-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="64524-110">取得し、HTTP GET メソッドを使用して、要求されたデータを表示`OnGetAsync`です。</span><span class="sxs-lookup"><span data-stu-id="64524-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="64524-111">HTTP POST メソッドを使用してデータへの変更を保存`OnPostAsync`です。</span><span class="sxs-lookup"><span data-stu-id="64524-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="64524-112">インデックスおよび詳細ページが取得し、HTTP GET メソッドを使用して、要求されたデータを表示`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="64524-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="64524-113">FirstOrDefaultAsync で SingleOrDefaultAsync を置き換えます</span><span class="sxs-lookup"><span data-stu-id="64524-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="64524-114">生成されたコードを使用して[SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)を要求されたエンティティを取得します。</span><span class="sxs-lookup"><span data-stu-id="64524-114">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="64524-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) 1 つのエンティティのフェッチでより効率的です。</span><span class="sxs-lookup"><span data-stu-id="64524-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="64524-116">コードがあることを確認する必要がありますいない限り、クエリから返される 1 つ以上のエンティティはありませんがあります。</span><span class="sxs-lookup"><span data-stu-id="64524-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="64524-117">`SingleOrDefaultAsync`多くのデータをフェッチし、不要な作業を行います。</span><span class="sxs-lookup"><span data-stu-id="64524-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="64524-118">`SingleOrDefaultAsync`フィルター部品に適合する 1 つ以上のエンティティがある場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="64524-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="64524-119">`FirstOrDefaultAsync`フィルター部品に適合する 1 つ以上のエンティティがある場合は、例外がスローされません。</span><span class="sxs-lookup"><span data-stu-id="64524-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="64524-120">グローバルに置き換える`SingleOrDefaultAsync`で`FirstOrDefaultAsync`です。</span><span class="sxs-lookup"><span data-stu-id="64524-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="64524-121">`SingleOrDefaultAsync`5 桁で使用されます。</span><span class="sxs-lookup"><span data-stu-id="64524-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="64524-122">`OnGetAsync`[詳細] ページにします。</span><span class="sxs-lookup"><span data-stu-id="64524-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="64524-123">`OnGetAsync`および`OnPostAsync`編集および削除のページにします。</span><span class="sxs-lookup"><span data-stu-id="64524-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="64524-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="64524-124">FindAsync</span></span>

<span data-ttu-id="64524-125">スキャフォールディングのコードの多くで[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___)の代わりに使用できる`FirstOrDefaultAsync`または`SingleOrDefaultAsync`です。</span><span class="sxs-lookup"><span data-stu-id="64524-125">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="64524-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="64524-126">`FindAsync`:</span></span>

* <span data-ttu-id="64524-127">主キー (PK) を持つエンティティを検索します。</span><span class="sxs-lookup"><span data-stu-id="64524-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="64524-128">主キーを持つエンティティはコンテキストによって追跡されているが場合、は、要求なし、DB に返されます。</span><span class="sxs-lookup"><span data-stu-id="64524-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="64524-129">単純で簡潔なです。</span><span class="sxs-lookup"><span data-stu-id="64524-129">Is simple and concise.</span></span>
* <span data-ttu-id="64524-130">1 つのエンティティの検索に最適化されます。</span><span class="sxs-lookup"><span data-stu-id="64524-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="64524-131">できますが、一部の状況でパフォーマンスの利点があります、通常の web シナリオの再生になることはほとんどありません。</span><span class="sxs-lookup"><span data-stu-id="64524-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="64524-132">暗黙的に使用[FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)の代わりに[SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)です。</span><span class="sxs-lookup"><span data-stu-id="64524-132">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="64524-133">他のエンティティを追加するかどうかは、検索に不要になったが、適切なします。</span><span class="sxs-lookup"><span data-stu-id="64524-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="64524-134">つまりに検索を破棄し、アプリの進行状況に応じて、クエリに移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="64524-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="64524-135">[詳細] ページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="64524-135">Customize the Details page</span></span>

<span data-ttu-id="64524-136">参照`Pages/Students`ページ。</span><span class="sxs-lookup"><span data-stu-id="64524-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="64524-137">**編集**、**詳細**、および**削除**へのリンクがによって生成される、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)で、*ページ/受講者/Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="64524-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="64524-138">詳細情報のリンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="64524-138">Select a Details link.</span></span> <span data-ttu-id="64524-139">URL の形式は、`http://localhost:5000/Students/Details?id=2`です。</span><span class="sxs-lookup"><span data-stu-id="64524-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="64524-140">クエリ文字列を使用して、学生 ID が渡されます (`?id=2`)。</span><span class="sxs-lookup"><span data-stu-id="64524-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="64524-141">更新を使用するには、編集、詳細、および Razor ページの削除、`"{id:int}"`ルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="64524-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="64524-142">これらの各ページのページ ディレクティブを `@page` から `@page "{id:int}"` に変更します。</span><span class="sxs-lookup"><span data-stu-id="64524-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="64524-143">"{Id: int}"のルート テンプレートを持つページへの要求**いない**整数ルート値を返します、HTTP 404 (見つかりません) エラーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="64524-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="64524-144">たとえば、 `http://localhost:5000/Students/Details` 404 エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="64524-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="64524-145">ID を省略するには、次のように `?` をルート制約に追加します。</span><span class="sxs-lookup"><span data-stu-id="64524-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="64524-146">アプリを実行、詳細リンクをクリックして、URL がルート データとして ID を渡すことを確認してください (`http://localhost:5000/Students/Details/2`)。</span><span class="sxs-lookup"><span data-stu-id="64524-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="64524-147">グローバルに変更しない`@page`に`@page "{id:int}"`ホームへのリンクの解除を行って、およびページを作成します。</span><span class="sxs-lookup"><span data-stu-id="64524-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="64524-148">関連するデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="64524-148">Add related data</span></span>

<span data-ttu-id="64524-149">受講者インデックス ページのスキャフォールディング コードが含まれていない、`Enrollments`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="64524-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="64524-150">このセクションの内容で、`Enrollments`コレクションは、詳細ページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="64524-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="64524-151">`OnGetAsync`メソッドの*Pages/Students/Details.cshtml.cs*を使用して、 `FirstOrDefaultAsync` 、1 つを取得する方法を`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="64524-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="64524-152">次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="64524-152">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="64524-153">`Include`と`ThenInclude`メソッドで発生する読み込みコンテキスト、`Student.Enrollments`ナビゲーション プロパティ、および各登録内で、`Enrollment.Course`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="64524-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="64524-154">これらのメソッドは、データの読み取りに関連するチュートリアル」で詳しく examinied はします。</span><span class="sxs-lookup"><span data-stu-id="64524-154">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="64524-155">`AsNoTracking`メソッドによって返されるエンティティは、現在のコンテキストでは更新されない時のシナリオでパフォーマンスを向上します。</span><span class="sxs-lookup"><span data-stu-id="64524-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="64524-156">`AsNoTracking`このチュートリアルで後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="64524-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="64524-157">[詳細] ページの関連する登録を表示します。</span><span class="sxs-lookup"><span data-stu-id="64524-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="64524-158">開いている*Pages/Students/Details.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="64524-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="64524-159">登録の一覧を表示する次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="64524-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="64524-160">コードを貼り付けた後でコードのインデントが正しくない場合は、修正して CTRL-D-K キーを押します。</span><span class="sxs-lookup"><span data-stu-id="64524-160">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="64524-161">上記のコードをループ内のエンティティ、`Enrollments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="64524-161">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="64524-162">各登録の場合は、コースのタイトルとグレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="64524-162">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="64524-163">格納されているコース エンティティからコース タイトルが取得される、`Course`登録エンティティのナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="64524-163">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="64524-164">アプリを実行する、選択、**受講者**タブをクリックし、をクリックして、**詳細**受講者をリンクします。</span><span class="sxs-lookup"><span data-stu-id="64524-164">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="64524-165">Courses に、選択した学生の成績の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="64524-165">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="64524-166">更新プログラムの作成 ページ</span><span class="sxs-lookup"><span data-stu-id="64524-166">Update the Create page</span></span>

<span data-ttu-id="64524-167">更新プログラム、`OnPostAsync`メソッド*Pages/Students/Create.cshtml.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="64524-167">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="64524-168">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="64524-168">TryUpdateModelAsync</span></span>

<span data-ttu-id="64524-169">確認、 [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_)コード。</span><span class="sxs-lookup"><span data-stu-id="64524-169">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="64524-170">上記のコードで`TryUpdateModelAsync<Student>`を更新しようとする、`emptyStudent`オブジェクトからポストされたフォーム値を使用して、 [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext)プロパティに、 [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0)です。</span><span class="sxs-lookup"><span data-stu-id="64524-170">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="64524-171">`TryUpdateModelAsync`プロパティを一覧表示の更新のみ (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="64524-171">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="64524-172">上記のサンプル。</span><span class="sxs-lookup"><span data-stu-id="64524-172">In the preceding sample:</span></span>

* <span data-ttu-id="64524-173">2 番目の引数 (` "student", // Prefix`) は、プレフィックスを使用して値を検索します。</span><span class="sxs-lookup"><span data-stu-id="64524-173">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="64524-174">大文字小文字を区別することはできません。</span><span class="sxs-lookup"><span data-stu-id="64524-174">It's not case sensitive.</span></span>
* <span data-ttu-id="64524-175">ポストされたフォーム値の型に変換、`Student`モデルを使用して[モデル バインディング](xref:mvc/models/model-binding#how-model-binding-works)です。</span><span class="sxs-lookup"><span data-stu-id="64524-175">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="64524-176">過剰ポスティング</span><span class="sxs-lookup"><span data-stu-id="64524-176">Overposting</span></span>

<span data-ttu-id="64524-177">使用して`TryUpdateModel`ポストされた値を持つフィールドを更新するにはセキュリティのベスト プラクティスを overposting を防ぐために、します。</span><span class="sxs-lookup"><span data-stu-id="64524-177">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="64524-178">たとえば、受講者エンティティが含まれています、`Secret`プロパティをこの web ページを更新または追加しないでください。</span><span class="sxs-lookup"><span data-stu-id="64524-178">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="64524-179">アプリを持っていない場合でも、 `Secret` Razor ページでは、ハッカーの作成/更新でフィールドが設定でした、`Secret`過剰ポスティングによっての値。</span><span class="sxs-lookup"><span data-stu-id="64524-179">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="64524-180">ハッカーでした、Fiddler などのツールを使用してまたは投稿に、一部の JavaScript を書き込み、`Secret`値を作成します。</span><span class="sxs-lookup"><span data-stu-id="64524-180">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="64524-181">学生インスタンスの作成時にモデル バインダーが使用されるフィールドの制限は、元のコードがありません。</span><span class="sxs-lookup"><span data-stu-id="64524-181">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="64524-182">値の指定されたハッカー、 `Secret` DB のフォーム フィールドを更新します。</span><span class="sxs-lookup"><span data-stu-id="64524-182">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="64524-183">次の図は、Fiddler ツールを追加する、`Secret`ポストされたフォーム値へのフィールド (に値"OverPost") です。</span><span class="sxs-lookup"><span data-stu-id="64524-183">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler シークレット フィールドの追加](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="64524-185">値に"OverPost"を正常に追加、`Secret`挿入された行のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="64524-185">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="64524-186">アプリケーション デザイナーを想定しなかった、`Secret`の作成 ページで設定されるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="64524-186">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="64524-187">ビュー モデル</span><span class="sxs-lookup"><span data-stu-id="64524-187">View model</span></span>

<span data-ttu-id="64524-188">ビュー モデルには、通常、アプリケーションによって使用されるモデルに含まれるプロパティのサブセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="64524-188">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="64524-189">アプリケーション モデルは、ドメイン モデルと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="64524-189">The application model is often called the domain model.</span></span> <span data-ttu-id="64524-190">ドメイン モデルには、通常、データベース内の対応するエンティティに必要なすべてのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="64524-190">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="64524-191">ビュー モデルには、UI 層 (たとえば、作成 ページ) に必要なプロパティのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="64524-191">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="64524-192">ビュー モデルだけでなく、一部のアプリケーションは、Razor ページのページのモデル クラスとブラウザー間でデータを受け渡すバインド モデルまたは入力モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="64524-192">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="64524-193">次の検討`Student`ビュー モデル。</span><span class="sxs-lookup"><span data-stu-id="64524-193">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="64524-194">ビューのモデルでは、overposting を防ぐために別の方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="64524-194">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="64524-195">ビュー モデルには、(表示) を表示または更新するプロパティのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="64524-195">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="64524-196">次のコードでは、`StudentVM`ビュー モデルに新しい学生を作成します。</span><span class="sxs-lookup"><span data-stu-id="64524-196">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="64524-197">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_)メソッドでは、このオブジェクトの値を設定を別の値を読み取って[PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="64524-197">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="64524-198">`SetValues`プロパティの名前の一致を使用します。</span><span class="sxs-lookup"><span data-stu-id="64524-198">`SetValues` uses property name matching.</span></span> <span data-ttu-id="64524-199">ビュー モデルの種類が、モデルの種類に関連している必要がある、一致するプロパティだけが必要です。</span><span class="sxs-lookup"><span data-stu-id="64524-199">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="64524-200">使用して`StudentVM`必要[CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml)に更新する`StudentVM`なく`Student`です。</span><span class="sxs-lookup"><span data-stu-id="64524-200">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="64524-201">Razor ページで、`PageModel`派生クラスは、ビュー モデルです。</span><span class="sxs-lookup"><span data-stu-id="64524-201">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="64524-202">更新プログラムの編集 ページ</span><span class="sxs-lookup"><span data-stu-id="64524-202">Update the Edit page</span></span>

<span data-ttu-id="64524-203">[編集] ページのページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="64524-203">Update the page model for the Edit page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="64524-204">コードの変更は、いくつかの例外の作成 ページに似ています。</span><span class="sxs-lookup"><span data-stu-id="64524-204">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="64524-205">`OnPostAsync`省略可能な`id`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="64524-205">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="64524-206">現在の受講者が、DB からフェッチされた空の学生を作成するのではなくです。</span><span class="sxs-lookup"><span data-stu-id="64524-206">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="64524-207">`FirstOrDefaultAsync`置き換えられました[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)です。</span><span class="sxs-lookup"><span data-stu-id="64524-207">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="64524-208">`FindAsync`主キーからエンティティを選択するときに、適切な選択です。</span><span class="sxs-lookup"><span data-stu-id="64524-208">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="64524-209">参照してください[FindAsync](#FindAsync)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="64524-209">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="64524-210">編集をテストし、ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="64524-210">Test the Edit and Create pages</span></span>

<span data-ttu-id="64524-211">作成し、いくつかの学生エンティティを編集します。</span><span class="sxs-lookup"><span data-stu-id="64524-211">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="64524-212">エンティティの状態</span><span class="sxs-lookup"><span data-stu-id="64524-212">Entity States</span></span>

<span data-ttu-id="64524-213">DB コンテキストは、メモリ内のエンティティが、DB 内の対応する行との同期であるかどうかの追跡を保持します。</span><span class="sxs-lookup"><span data-stu-id="64524-213">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="64524-214">DB コンテキストの同期情報の動作を決定するときに`SaveChanges`と呼びます。</span><span class="sxs-lookup"><span data-stu-id="64524-214">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="64524-215">たとえば、新しいエンティティが渡されたときにする、`Add`メソッドに設定されているエンティティの状態を`Added`です。</span><span class="sxs-lookup"><span data-stu-id="64524-215">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="64524-216">ときに`SaveChanges`が呼び出されると、データベース コンテキストは、SQL の INSERT コマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="64524-216">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="64524-217">次の状態のいずれかでエンティティがあります。</span><span class="sxs-lookup"><span data-stu-id="64524-217">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="64524-218">`Added`: エンティティは、DB にまだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="64524-218">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="64524-219">`SaveChanges`メソッドは、INSERT ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="64524-219">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="64524-220">`Unchanged`変更する必要があります: このエンティティで保存されます。</span><span class="sxs-lookup"><span data-stu-id="64524-220">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="64524-221">エンティティでは、データベースから読み取られるときにこの状態があります。</span><span class="sxs-lookup"><span data-stu-id="64524-221">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="64524-222">`Modified`: 一部またはすべてのエンティティのプロパティの値が変更されています。</span><span class="sxs-lookup"><span data-stu-id="64524-222">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="64524-223">`SaveChanges`メソッドは、UPDATE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="64524-223">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="64524-224">`Deleted`: エンティティは、削除のマークされています。</span><span class="sxs-lookup"><span data-stu-id="64524-224">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="64524-225">`SaveChanges`メソッドは、DELETE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="64524-225">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="64524-226">`Detached`: エンティティは、DB コンテキストによって追跡されているはありません。</span><span class="sxs-lookup"><span data-stu-id="64524-226">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="64524-227">デスクトップ アプリでは、通常状態の変更から自動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="64524-227">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="64524-228">エンティティの読み取り、変更を加えると、エンティティの状態に自動的に変更する`Modified`です。</span><span class="sxs-lookup"><span data-stu-id="64524-228">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="64524-229">呼び出す`SaveChanges`変更されたプロパティのみを更新する SQL UPDATE ステートメントを生成します。</span><span class="sxs-lookup"><span data-stu-id="64524-229">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="64524-230">Web アプリで、`DbContext`を読み取るエンティティと、ページが表示された後、データが破棄されて表示されます。</span><span class="sxs-lookup"><span data-stu-id="64524-230">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="64524-231">ときに、ページ`OnPostAsync`メソッドが呼び出されると、新しい web 要求が行われるの新しいインスタンスを使用して、`DbContext`です。</span><span class="sxs-lookup"><span data-stu-id="64524-231">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="64524-232">デスクトップの処理をシミュレートする新しいコンテキスト内のエンティティを再読み込みします。</span><span class="sxs-lookup"><span data-stu-id="64524-232">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="64524-233">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="64524-233">Update the Delete page</span></span>

<span data-ttu-id="64524-234">ここでは、カスタム エラーを実装するコードを追加メッセージへの呼び出し`SaveChanges`は失敗します。</span><span class="sxs-lookup"><span data-stu-id="64524-234">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="64524-235">考えられるエラー メッセージを格納する文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="64524-235">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="64524-236">`OnGetAsync` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="64524-236">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="64524-237">上記のコードには、省略可能なパラメーターが含まれています。`saveChangesError`です。</span><span class="sxs-lookup"><span data-stu-id="64524-237">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="64524-238">`saveChangesError`student オブジェクトの削除に失敗した後、メソッドが呼び出されたかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="64524-238">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="64524-239">一時的なネットワークの問題があるため、削除操作が失敗可能性があります。</span><span class="sxs-lookup"><span data-stu-id="64524-239">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="64524-240">ネットワークの一時的なエラーは、クラウドで可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="64524-240">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="64524-241">`saveChangesError`ときに、false 削除ページ`OnGetAsync`UI から呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="64524-241">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="64524-242">ときに`OnGetAsync`によって呼び出される`OnPostAsync`(ため、削除操作が失敗しました)、`saveChangesError`パラメーターが true です。</span><span class="sxs-lookup"><span data-stu-id="64524-242">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="64524-243">Delete ページ OnPostAsync メソッド</span><span class="sxs-lookup"><span data-stu-id="64524-243">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="64524-244">置換、`OnPostAsync`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="64524-244">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="64524-245">上記のコードは、選択したエンティティを取得し、呼び出して、`Remove`エンティティの状態を設定するメソッドを`Deleted`です。</span><span class="sxs-lookup"><span data-stu-id="64524-245">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="64524-246">ときに`SaveChanges`が呼び出されると、SQL の DELETE コマンドを生成します。</span><span class="sxs-lookup"><span data-stu-id="64524-246">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="64524-247">場合`Remove`は失敗します。</span><span class="sxs-lookup"><span data-stu-id="64524-247">If `Remove` fails:</span></span>

* <span data-ttu-id="64524-248">DB 例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="64524-248">The DB exception is caught.</span></span>
* <span data-ttu-id="64524-249">Delete ページ`OnGetAsync`メソッドが呼び出された`saveChangesError=true`です。</span><span class="sxs-lookup"><span data-stu-id="64524-249">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="64524-250">Delete Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="64524-250">Update the Delete Razor Page</span></span>

<span data-ttu-id="64524-251">Razor ページの削除するには、次の強調表示されたエラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="64524-251">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="64524-252">削除をテストします。</span><span class="sxs-lookup"><span data-stu-id="64524-252">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="64524-253">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="64524-253">Common errors</span></span>

<span data-ttu-id="64524-254">学生/ホームまたはその他のリンクは機能しません。</span><span class="sxs-lookup"><span data-stu-id="64524-254">Student/Home or other links don't work:</span></span>

<span data-ttu-id="64524-255">Razor ページが含まれていますが、正しいことを確認`@page`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="64524-255">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="64524-256">など、受講者/ホーム Razor ページする必要があります**いない**ルート テンプレートを含めます。</span><span class="sxs-lookup"><span data-stu-id="64524-256">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="64524-257">各 Razor ページを含める必要があります、`@page`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="64524-257">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="64524-258">[前へ](xref:data/ef-rp/intro)
[次へ](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="64524-258">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>