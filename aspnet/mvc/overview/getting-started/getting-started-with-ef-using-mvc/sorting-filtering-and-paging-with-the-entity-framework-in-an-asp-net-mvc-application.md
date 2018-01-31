---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "並べ替え、フィルター、および Entity Framework、ASP.NET MVC アプリケーションでのページング |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d54c0e133bc2f6f2021821dc16cdf86cc23a5667
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="f56ac-103">並べ替え、フィルター、および Entity Framework、ASP.NET MVC アプリケーションでのページング</span><span class="sxs-lookup"><span data-stu-id="f56ac-103">Sorting, Filtering, and Paging with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="f56ac-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f56ac-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f56ac-105">[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="f56ac-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="f56ac-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="f56ac-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="f56ac-108">前のチュートリアルでは、基本的な CRUD 操作するための web ページのセットを実装`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-108">In the previous tutorial you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="f56ac-109">このチュートリアルでは、並べ替え、フィルター、およびページング機能を追加します、**受講者**インデックス ページです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-109">In this tutorial you'll add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="f56ac-110">単純なグループ化を実行するページを作成することもあります。</span><span class="sxs-lookup"><span data-stu-id="f56ac-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="f56ac-111">次の図は、ページがどのようにしたらを示します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="f56ac-112">列見出しとは、その列で並べ替えを行うユーザーがクリックするリンクです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="f56ac-113">列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序を切り替えます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="f56ac-115">受講者インデックス ページに列の並べ替えのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-115">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="f56ac-116">学生インデックス ページに並べ替えを追加するを変更、`Index`のメソッド、`Student`コント ローラーのコードを追加し、`Student`ビューにインデックスをします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-116">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="f56ac-117">並べ替え、インデックス メソッドに機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-117">Add Sorting Functionality to the Index Method</span></span>

<span data-ttu-id="f56ac-118">*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="f56ac-118">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="f56ac-119">このコードを受け取る、 `sortOrder` URL 内のクエリ文字列からのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="f56ac-120">クエリ文字列の値は、アクション メソッドにパラメーターとして、ASP.NET MVC によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-120">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="f56ac-121">パラメーターが"Name"または「日付」、必要に応じて後にアンダー スコアと降順の順序を指定するには、"desc"文字列を文字列になります。</span><span class="sxs-lookup"><span data-stu-id="f56ac-121">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="f56ac-122">既定の並べ替え順序は昇順です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-122">The default sort order is ascending.</span></span>

<span data-ttu-id="f56ac-123">インデックス ページが要求されると、最初にクエリ文字列はありません。</span><span class="sxs-lookup"><span data-stu-id="f56ac-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="f56ac-124">受講者がで昇順に表示される`LastName`、フォール スルー大文字小文字によって設定される既定値は、`switch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-124">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="f56ac-125">ユーザーが列見出し、ハイパーリンク、適切な`sortOrder`クエリ文字列の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="f56ac-126">2 つ`ViewBag`変数を使用するビューは、適切なクエリ文字列の値を列見出しのハイパーリンクを構成できます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-126">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="f56ac-127">これらは、三項ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-127">These are ternary statements.</span></span> <span data-ttu-id="f56ac-128">最初の 1 つを指定する場合、`sortOrder`パラメーターが null または空で、`ViewBag.NameSortParm`に設定する必要があります"名\_desc"、それ以外の空の文字列に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f56ac-128">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="f56ac-129">これら 2 つのステートメントでは、ビュー、列見出しのハイパーリンクの次のように設定を有効にします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="f56ac-130">現在の並べ替え順序</span><span class="sxs-lookup"><span data-stu-id="f56ac-130">Current sort order</span></span> | <span data-ttu-id="f56ac-131">名の最後のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="f56ac-131">Last Name Hyperlink</span></span> | <span data-ttu-id="f56ac-132">日付のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="f56ac-132">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f56ac-133">最後の名前昇順</span><span class="sxs-lookup"><span data-stu-id="f56ac-133">Last Name ascending</span></span> | <span data-ttu-id="f56ac-134">descending</span><span class="sxs-lookup"><span data-stu-id="f56ac-134">descending</span></span> | <span data-ttu-id="f56ac-135">ascending</span><span class="sxs-lookup"><span data-stu-id="f56ac-135">ascending</span></span> |
| <span data-ttu-id="f56ac-136">最後の名前が降順</span><span class="sxs-lookup"><span data-stu-id="f56ac-136">Last Name descending</span></span> | <span data-ttu-id="f56ac-137">ascending</span><span class="sxs-lookup"><span data-stu-id="f56ac-137">ascending</span></span> | <span data-ttu-id="f56ac-138">ascending</span><span class="sxs-lookup"><span data-stu-id="f56ac-138">ascending</span></span> |
| <span data-ttu-id="f56ac-139">日付の昇順</span><span class="sxs-lookup"><span data-stu-id="f56ac-139">Date ascending</span></span> | <span data-ttu-id="f56ac-140">ascending</span><span class="sxs-lookup"><span data-stu-id="f56ac-140">ascending</span></span> | <span data-ttu-id="f56ac-141">descending</span><span class="sxs-lookup"><span data-stu-id="f56ac-141">descending</span></span> |
| <span data-ttu-id="f56ac-142">日付 (降順)</span><span class="sxs-lookup"><span data-stu-id="f56ac-142">Date descending</span></span> | <span data-ttu-id="f56ac-143">ascending</span><span class="sxs-lookup"><span data-stu-id="f56ac-143">ascending</span></span> | <span data-ttu-id="f56ac-144">ascending</span><span class="sxs-lookup"><span data-stu-id="f56ac-144">ascending</span></span> |

<span data-ttu-id="f56ac-145">メソッドを使用して[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)を並べ替え列を指定します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-145">The method uses [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) to specify the column to sort by.</span></span> <span data-ttu-id="f56ac-146">コードを作成、 [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)する前に変数、`switch`ステートメントでは、変更で、`switch`ステートメント、および呼び出し、`ToList`メソッドした後に、`switch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-146">The code creates an [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="f56ac-147">作成および変更するときに`IQueryable`変数、クエリがないデータベースに送信します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="f56ac-148">変換するまでに、クエリが実行されていません、`IQueryable`などのメソッドを呼び出すことで、コレクションにオブジェクト`ToList`です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="f56ac-149">そのため、このコードなりますまで実行されない 1 つのクエリで、`return View`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="f56ac-150">それぞれの並べ替え順序のさまざまな LINQ ステートメントを記述する代わりに、LINQ ステートメントを動的に作成できます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-150">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="f56ac-151">動的な LINQ の概要については、次を参照してください。[動的 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-151">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="f56ac-152">列見出しを学生インデックス ビューへのハイパーリンクの追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-152">Add Column Heading Hyperlinks to the Student Index View</span></span>

<span data-ttu-id="f56ac-153">*Views\Student\Index.cshtml*、置換、`<tr>`と`<th>`の見出し行を強調表示されたコードが要素。</span><span class="sxs-lookup"><span data-stu-id="f56ac-153">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

<span data-ttu-id="f56ac-154">このコード内の情報を使用して、`ViewBag`適切なクエリでハイパーリンクを設定するプロパティの文字列値です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-154">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="f56ac-155">ページを実行し、をクリックして、**姓**と**登録日**を並べ替えることを確認する列見出しが動作します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-155">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="f56ac-157">クリックした後、**姓**見出し、受講者が降順で表示される最後の名前。</span><span class="sxs-lookup"><span data-stu-id="f56ac-157">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="f56ac-158">受講者インデックス ページに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-158">Add a Search Box to the Students Index Page</span></span>

<span data-ttu-id="f56ac-159">受講者インデックス ページにフィルターを追加するにをビューにテキスト ボックスと [送信] ボタンを追加しで対応する変更を加え、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-159">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="f56ac-160">テキスト ボックスを使用すると、姓と名の最後のフィールドで検索する文字列を入力できます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-160">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="f56ac-161">Index メソッドにフィルター処理機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-161">Add Filtering Functionality to the Index Method</span></span>

<span data-ttu-id="f56ac-162">*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード (変更が強調表示されます)。</span><span class="sxs-lookup"><span data-stu-id="f56ac-162">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="f56ac-163">追加した、`searchString`パラメーターを`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-163">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="f56ac-164">インデックス ビューに追加するテキスト ボックスから、検索する文字列値を受信します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-164">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="f56ac-165">また、LINQ ステートメントに追加した、`where`名または姓を持つ検索文字列が含まれています。 受講者のみが選択した句。</span><span class="sxs-lookup"><span data-stu-id="f56ac-165">You've also added to the LINQ statement a `where` clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="f56ac-166">ステートメントを追加する、[場所](https://msdn.microsoft.com/library/bb535040.aspx)句は検索対象の値がある場合にのみ実行します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-166">The statement that adds the [where](https://msdn.microsoft.com/library/bb535040.aspx) clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="f56ac-167">多くの場合は、Entity Framework のエンティティ セットまたはメモリ内コレクションの拡張メソッドとして同じメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-167">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="f56ac-168">結果は、通常は同じですが、場合によっては異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f56ac-168">The results are normally the same but in some cases may be different.</span></span>
> 
> <span data-ttu-id="f56ac-169">.NET Framework の実装など、`Contains`メソッドは、空の文字列を渡しますが、Entity Framework provider for SQL Server Compact 4.0 には、空の文字列には、0 行が返されます。 すべての行を返します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-169">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="f56ac-170">そのため、例のコードで (配置、`Where`内のステートメント、`if`ステートメント) すべてのバージョンの SQL Server は、同じ結果を取得することを確認します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-170">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="f56ac-171">また、.NET Framework の実装の`Contains`メソッドは、既定では、大文字小文字の比較を実行しますが、エンティティ フレームワークの SQL Server プロバイダーは、既定では大文字と小文字の比較を実行します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-171">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="f56ac-172">そのため、呼び出し、`ToUpper`を明示的に大文字と小文字、テストを行うメソッドにより、結果を変更しないことを返す、リポジトリを使用するには、後でコードを変更するときに、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f56ac-172">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="f56ac-173">(を呼び出すと、`Contains`メソッドを`IEnumerable`.NET Framework の実装を取得する、コレクション以外のときに呼び出す、`IQueryable`オブジェクト、データベース プロバイダーの実装を取得します)。</span><span class="sxs-lookup"><span data-stu-id="f56ac-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
> 
> <span data-ttu-id="f56ac-174">Null の処理が異なる別のデータベース プロバイダーまたは使用する場合も可能性があります、`IQueryable`オブジェクトと比較して使用すると、`IEnumerable`コレクション。</span><span class="sxs-lookup"><span data-stu-id="f56ac-174">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="f56ac-175">たとえば、一部のシナリオで、`Where`などの条件`table.Column != 0`を持つ列が返されない可能性があります`null`値として。</span><span class="sxs-lookup"><span data-stu-id="f56ac-175">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="f56ac-176">詳細については、次を参照してください。 ['where' 句で null の変数が正しく処理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-176">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>


### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="f56ac-177">学生インデックス ビューに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="f56ac-178">*Views\Student\Index.cshtml*、開始する直前に強調表示されたコードを追加`table`タグ、キャプションのテキスト ボックスを作成するために、**検索**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-178">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

<span data-ttu-id="f56ac-179">ページを実行し、検索文字列を入力し をクリックして**検索**フィルタ リングが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-179">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="f56ac-181">URL には「、」検索文字列このページのブックマークを設定した場合はありません一覧を取得するフィルター選択されたブックマークを使用する場合は意味にはが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-181">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="f56ac-182">これに適用されますも列の並べ替えリンク全体の一覧が並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-182">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="f56ac-183">変更、**検索**チュートリアルの後半で、フィルター条件のクエリ文字列を使用するボタンです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-183">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging-to-the-students-index-page"></a><span data-ttu-id="f56ac-184">ページングを受講者インデックス ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-184">Add Paging to the Students Index Page</span></span>

<span data-ttu-id="f56ac-185">ページングを受講者インデックス ページに追加するをインストールして起動されます、 **PagedList.Mvc** NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-185">To add paging to the Students Index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="f56ac-186">他の変更を加えるされます、`Index`メソッドへのページングのリンクを追加し、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="f56ac-186">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="f56ac-187">**PagedList.Mvc**多くの優れたページングと ASP.NET MVC 用のパッケージの並べ替えの 1 つであり、ここでを目的として他のオプションより、インデックスの推奨ではなく、例としてのみです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-187">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span> <span data-ttu-id="f56ac-188">次の図は、ページングは、リンクを示します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-188">The following illustration shows the paging links.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="f56ac-190">PagedList.MVC NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-190">Install the PagedList.MVC NuGet Package</span></span>

<span data-ttu-id="f56ac-191">NuGet **PagedList.Mvc**パッケージを自動的にインストール、 **PagedList**依存関係としてパッケージします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-191">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="f56ac-192">**PagedList**パッケージのインストール、`PagedList`のコレクションの種類と拡張子メソッド`IQueryable`と`IEnumerable`コレクション。</span><span class="sxs-lookup"><span data-stu-id="f56ac-192">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="f56ac-193">拡張メソッド内のデータの単一ページを作成する、`PagedList`コレクションのうち、`IQueryable`または`IEnumerable`、および`PagedList`コレクションでいくつかのプロパティとページングを容易にするメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-193">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="f56ac-194">**PagedList.Mvc**ページング ボタンを表示するページング ヘルパーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-194">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

<span data-ttu-id="f56ac-195">**ツール**メニューの **ライブラリ パッケージ マネージャー**し**Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-195">From the **Tools** menu, select **Library Package Manager** and then **Package Manager Console**.</span></span>

<span data-ttu-id="f56ac-196">**パッケージ マネージャー コンソール** ウィンドウを確認してください、**パッケージ ソース**は**nuget.org**と**既定のプロジェクト**は、**ContosoUniversity**、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-196">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

`Install-Package PagedList.Mvc`

![PagedList.Mvc をインストールします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="f56ac-198">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-198">Build the project.</span></span> 

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="f56ac-199">Index メソッドにページング機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-199">Add Paging Functionality to the Index Method</span></span>

<span data-ttu-id="f56ac-200">*Controllers\StudentController.cs*、追加、`using`のステートメント、`PagedList`名前空間。</span><span class="sxs-lookup"><span data-stu-id="f56ac-200">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="f56ac-201">`Index` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-201">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

<span data-ttu-id="f56ac-202">このコードを追加、`page`パラメーター、現在の並べ替え順序パラメーターおよびメソッドのシグネチャに現在のフィルター パラメーター。</span><span class="sxs-lookup"><span data-stu-id="f56ac-202">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="f56ac-203">最初に、ページが表示されたら、またはユーザーが、ページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。</span><span class="sxs-lookup"><span data-stu-id="f56ac-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span> <span data-ttu-id="f56ac-204">ページング リンクをクリックした場合、`page`変数は、表示するページ番号が格納されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-204">If a paging link is clicked, the `page` variable will contain the page number to display.</span></span>

<span data-ttu-id="f56ac-205">A`ViewBag`プロパティは、このページング中に同じ並べ替え順序を維持するために、ページングのリンクに含める必要がありますので、現在の並べ替え順序で使用してビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-205">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="f56ac-206">別のプロパティ、 `ViewBag.CurrentFilter`、現在のフィルター文字列に、ビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-206">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="f56ac-207">ページング、中に、フィルターの設定を維持するために、ページングのリンクでこの値を含める必要があるし、ページが表示されときに、テキスト ボックスに復元する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f56ac-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="f56ac-208">ページングの中に検索文字列を変更する場合は、新しいフィルターが、別のデータを表示するため、ページを 1 にリセットします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="f56ac-209">テキスト ボックスに値を入力し、[送信] ボタンが押された検索文字列が変更されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-209">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="f56ac-210">その場合は、`searchString`パラメーターが null ではありません。</span><span class="sxs-lookup"><span data-stu-id="f56ac-210">In that case, the `searchString` parameter is not null.</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

<span data-ttu-id="f56ac-211">メソッドの最後に、`ToPagedList`拡張メソッド、受講者を`IQueryable`オブジェクトは、student クエリをページングをサポートするコレクション型の生徒の 1 つのページに変換します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-211">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="f56ac-212">その 1 つのページの受講者がビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-212">That single page of students is then passed to the view:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="f56ac-213">`ToPagedList`メソッドは、ページ数を取得します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-213">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="f56ac-214">2 つの疑問符を表す、 [null 合体演算子](https://msdn.microsoft.com/library/ms173224.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-214">The two question marks represent the [null-coalescing operator](https://msdn.microsoft.com/library/ms173224.aspx).</span></span> <span data-ttu-id="f56ac-215">Null 合体演算子を null 許容型以外の既定値を定義します式`(page ?? 1)`の値を返すことを意味`page`かどうかは、値を持つまたは 1 を返す`page`が null です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="f56ac-216">学生インデックス ビューにページングのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-216">Add Paging Links to the Student Index View</span></span>

<span data-ttu-id="f56ac-217">*Views\Student\Index.cshtml*、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-217">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="f56ac-218">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-218">The changes are highlighted.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

<span data-ttu-id="f56ac-219">`@model`ページの上部にあるステートメントは、ビューが現在を取得するを指定します、`PagedList`オブジェクトの代わりに、`List`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f56ac-219">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

<span data-ttu-id="f56ac-220">`using`の声明`PagedList.Mvc`ページング ボタンの MVC ヘルパーへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-220">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

<span data-ttu-id="f56ac-221">コードのオーバー ロードを使用して[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)を指定することができる[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-221">The code uses an overload of [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) that allows it to specify [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

<span data-ttu-id="f56ac-222">既定値[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)をパラメーターとして渡される HTTP メッセージの本文では、URL ではなくクエリ文字列は、投稿内容がフォームのデータを送信します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-222">The default [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="f56ac-223">HTTP GET を指定すると、フォームのデータに渡されます URL クエリ文字列としてユーザーの URL をブックマークすることができます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-223">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="f56ac-224">[HTTP GET を使用するための W3C ガイドライン](http://www.w3.org/2001/tag/doc/whenToUseGet.html)アクションが、更新プログラムにならない場合に GET を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-224">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="f56ac-225">テキスト ボックスは、新しいページをクリックすると、現在の検索文字列を表示できるように、現在の検索文字列で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-225">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

<span data-ttu-id="f56ac-226">列ヘッダーへのリンクは、フィルターの結果内で、ユーザーが並べ替えられるように、コント ローラーに現在の検索文字列を渡すクエリ文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-226">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

<span data-ttu-id="f56ac-227">現在のページと合計ページ数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-227">The current page and total number of pages are displayed.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

<span data-ttu-id="f56ac-228">表示するページがない場合は、「0 の 0 ページ」が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-228">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="f56ac-229">(その場合は、ページ番号がページ数より大きいため`Model.PageNumber`1 に設定されてと`Model.PageCount`は 0 です)。</span><span class="sxs-lookup"><span data-stu-id="f56ac-229">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

<span data-ttu-id="f56ac-230">ページング ボタンが表示されます、`PagedListPager`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="f56ac-230">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

<span data-ttu-id="f56ac-231">`PagedListPager`ヘルパーは、複数の Url を含むおよびスタイル設定、カスタマイズ可能なオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-231">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="f56ac-232">詳細については、次を参照してください。 [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList)については、GitHub サイトです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-232">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

<span data-ttu-id="f56ac-233">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-233">Run the page.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="f56ac-235">ページング動作のことを確認する異なる並べ替え順のページングのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-235">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="f56ac-236">検索文字列を入力し、ページング ページングも正常に動作する並べ替えとフィルター処理のことを確認するには、もう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="f56ac-236">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="f56ac-237">作成する、受講者の統計情報を表示するページについて</span><span class="sxs-lookup"><span data-stu-id="f56ac-237">Create an About Page That Shows Student Statistics</span></span>

<span data-ttu-id="f56ac-238">Contoso 大学 web サイトのページについて、登録日付ごとにどのように多くの学生が登録に表示されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-238">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="f56ac-239">これには、グループにグループ化と簡単な計算が必要です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-239">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="f56ac-240">これを実現するには、次を行います。</span><span class="sxs-lookup"><span data-stu-id="f56ac-240">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="f56ac-241">ビューに渡す必要があるデータのビュー モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-241">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="f56ac-242">変更、`About`メソッドで、`Home`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="f56ac-242">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="f56ac-243">変更、`About`ビュー。</span><span class="sxs-lookup"><span data-stu-id="f56ac-243">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="f56ac-244">ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-244">Create the View Model</span></span>

<span data-ttu-id="f56ac-245">作成、 *ViewModels*プロジェクト フォルダー内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="f56ac-245">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="f56ac-246">クラス ファイルを追加、そのフォルダー内*EnrollmentDateGroup.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-246">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="f56ac-247">Home コント ローラーの変更します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-247">Modify the Home Controller</span></span>

<span data-ttu-id="f56ac-248">*HomeController.cs*、次の追加`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="f56ac-248">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="f56ac-249">クラスの始め中かっこの直後に、データベース コンテキストのクラスの変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-249">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

<span data-ttu-id="f56ac-250">`About` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-250">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="f56ac-251">LINQ ステートメントの登録日で学生エンティティをグループ化、各グループ内のエンティティの数を計算し、結果のコレクションに格納`EnrollmentDateGroup`モデル オブジェクトを表示します。</span><span class="sxs-lookup"><span data-stu-id="f56ac-251">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

<span data-ttu-id="f56ac-252">追加、`Dispose`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f56ac-252">Add a `Dispose` method:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="f56ac-253">変更、ビューについて</span><span class="sxs-lookup"><span data-stu-id="f56ac-253">Modify the About View</span></span>

<span data-ttu-id="f56ac-254">コードで置き換え、 *Views\Home\About.cshtml*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="f56ac-254">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

<span data-ttu-id="f56ac-255">アプリケーションを実行し、をクリックして、**に関する**リンクします。</span><span class="sxs-lookup"><span data-stu-id="f56ac-255">Run the app and click the **About** link.</span></span> <span data-ttu-id="f56ac-256">登録の日付ごとの生徒の数は、テーブルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-256">The count of students for each enrollment date is displayed in a table.</span></span>

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="f56ac-258">まとめ</span><span class="sxs-lookup"><span data-stu-id="f56ac-258">Summary</span></span>

<span data-ttu-id="f56ac-259">このチュートリアルでは、データ モデルを作成して基本的な CRUD、並べ替え、フィルター、ページング、および機能をグループ化を実装する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="f56ac-259">In this tutorial you've seen how to create a data model and implement basic CRUD, sorting, filtering, paging, and grouping functionality.</span></span> <span data-ttu-id="f56ac-260">次のチュートリアルでは、データ モデルを展開してより高度なトピックを見るが始めます。</span><span class="sxs-lookup"><span data-stu-id="f56ac-260">In the next tutorial you'll begin looking at more advanced topics by expanding the data model.</span></span>

<span data-ttu-id="f56ac-261">このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="f56ac-261">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="f56ac-262">新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-262">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="f56ac-263">その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="f56ac-263">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f56ac-264">[前へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[次へ](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="f56ac-264">[Previous](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Next](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>