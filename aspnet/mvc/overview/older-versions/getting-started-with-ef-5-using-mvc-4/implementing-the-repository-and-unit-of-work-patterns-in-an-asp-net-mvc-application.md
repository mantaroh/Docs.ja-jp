---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (9 10 の) リポジトリおよび作業パターンの単位を実装する |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 02b1de31b9513247facc92bc6b72247865d176f9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a><span data-ttu-id="d4fdd-103">ASP.NET MVC アプリケーション (9 10 の) リポジトリおよび作業パターンの単位を実装します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-103">Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application (9 of 10)</span></span>
====================
<span data-ttu-id="d4fdd-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d4fdd-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="d4fdd-105">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="d4fdd-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="d4fdd-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="d4fdd-108">一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="d4fdd-109">解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="d4fdd-110">一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="d4fdd-111">一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="d4fdd-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="d4fdd-112">前のチュートリアルでの冗長なコードを削減する継承を使用して、`Student`と`Instructor`エンティティ クラスです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-112">In the previous tutorial you used inheritance to reduce redundant code in the `Student` and `Instructor` entity classes.</span></span> <span data-ttu-id="d4fdd-113">このチュートリアルでの CRUD 操作のリポジトリと作業パターンの単位を使用するいくつかの方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-113">In this tutorial you'll see some ways to use the repository and unit of work patterns for CRUD operations.</span></span> <span data-ttu-id="d4fdd-114">このいずれかで、前のチュートリアルと同様に、コードが新しいページを作成するのではなく、既に作成したページと機能の仕方を変更します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-114">As in the previous tutorial, in this one you'll change the way your code works with pages you already created rather than creating new pages.</span></span>

## <a name="the-repository-and-unit-of-work-patterns"></a><span data-ttu-id="d4fdd-115">リポジトリと作業パターンの単位</span><span class="sxs-lookup"><span data-stu-id="d4fdd-115">The Repository and Unit of Work Patterns</span></span>

<span data-ttu-id="d4fdd-116">リポジトリと作業パターンの単位は、データ アクセス層とアプリケーションのビジネス ロジック層の間で抽象化レイヤーを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-116">The repository and unit of work patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="d4fdd-117">これらのパターンを実装して、変更によって、データ ストアにアプリケーションを分離できます、自動化された単体テストまたはテスト駆動開発 (TDD) に容易にすることができます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-117">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span>

<span data-ttu-id="d4fdd-118">このチュートリアルでは各エンティティ タイプのリポジトリ クラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-118">In this tutorial you'll implement a repository class for each entity type.</span></span> <span data-ttu-id="d4fdd-119">`Student`リポジトリ インターフェイスおよびリポジトリ クラスを作成するエンティティ型。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-119">For the `Student` entity type you'll create a repository interface and a repository class.</span></span> <span data-ttu-id="d4fdd-120">コント ローラーでリポジトリをインスタンス化するときに、コント ローラーがリポジトリ インターフェイスを実装する任意のオブジェクトへの参照を受け入れるように、インターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-120">When you instantiate the repository in your controller, you'll use the interface so that the controller will accept a reference to any object that implements the repository interface.</span></span> <span data-ttu-id="d4fdd-121">Web サーバーの下で、コント ローラーを実行すると、Entity Framework と連携する、リポジトリを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-121">When the controller runs under a web server, it receives a repository that works with the Entity Framework.</span></span> <span data-ttu-id="d4fdd-122">単体テスト クラスの下で、コント ローラーが実行されると、テストでは、メモリ内コレクションなどの簡単に操作できるように格納されているデータと連携する、リポジトリを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-122">When the controller runs under a unit test class, it receives a repository that works with data stored in a way that you can easily manipulate for testing, such as an in-memory collection.</span></span>

<span data-ttu-id="d4fdd-123">このチュートリアルで後ほど使用する複数のリポジトリとの作業のクラスの単体、`Course`と`Department`エンティティ型、`Course`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-123">Later in the tutorial you'll use multiple repositories and a unit of work class for the `Course` and `Department` entity types in the `Course` controller.</span></span> <span data-ttu-id="d4fdd-124">クラスの作業の単位は、それらすべてによって共有される 1 つのデータベース コンテキスト クラスを作成することで複数のリポジトリの作業を調整します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-124">The unit of work class coordinates the work of multiple repositories by creating a single database context class shared by all of them.</span></span> <span data-ttu-id="d4fdd-125">自動化された単体テストを実行できる場合は、作成し、これらのクラスが、同じ方法でインターフェイスを使用して、`Student`リポジトリです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-125">If you wanted to be able to perform automated unit testing, you'd create and use interfaces for these classes in the same way you did for the `Student` repository.</span></span> <span data-ttu-id="d4fdd-126">ただし、チュートリアルを簡潔にするを作成し、インターフェイスせずにこれらのクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-126">However, to keep the tutorial simple, you'll create and use these classes without interfaces.</span></span>

<span data-ttu-id="d4fdd-127">次の図は、コント ローラーとまったく使用しないリポジトリまたはパターンの作業の単位と比較してコンテキスト クラス間の関係を理解する 1 つの方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-127">The following illustration shows one way to conceptualize the relationships between the controller and context classes compared to not using the repository or unit of work pattern at all.</span></span>

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

<span data-ttu-id="d4fdd-129">このチュートリアルの一連の単体テストを作成できません。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-129">You won't create unit tests in this tutorial series.</span></span> <span data-ttu-id="d4fdd-130">概要については TDD リポジトリ パターンを使用して MVC アプリケーションで、次を参照してください。[チュートリアル: ASP.NET MVC で TDD を使用して](https://msdn.microsoft.com/library/ff847525.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-130">For an introduction to TDD with an MVC application that uses the repository pattern, see [Walkthrough: Using TDD with ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx).</span></span> <span data-ttu-id="d4fdd-131">リポジトリ パターンの詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-131">For more information about the repository pattern, see the following resources:</span></span>

- <span data-ttu-id="d4fdd-132">[リポジトリ パターン](https://msdn.microsoft.com/library/ff649690.aspx)msdn です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-132">[The Repository Pattern](https://msdn.microsoft.com/library/ff649690.aspx) on MSDN.</span></span>
- <span data-ttu-id="d4fdd-133">[Entity Framework 4.0 でリポジトリと作業単位のパターンを使用して](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)Entity Framework チームのブログです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-133">[Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) on the Entity Framework team blog.</span></span>
- <span data-ttu-id="d4fdd-134">[アジャイルの Entity Framework 4 リポジトリ](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)Julie Lerman のブログの投稿の系列です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-134">[Agile Entity Framework 4 Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) series of posts on Julie Lerman's blog.</span></span>
- <span data-ttu-id="d4fdd-135">[概要の HTML5 と jQuery アプリケーションでアカウントを構築](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin ブログ。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-135">[Building the Account at a Glance HTML5/jQuery Application](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) on Dan Wahlin's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="d4fdd-136">リポジトリと作業パターンの単位を実装する方法はたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-136">There are many ways to implement the repository and unit of work patterns.</span></span> <span data-ttu-id="d4fdd-137">クラスの作業の単位の有無は、リポジトリ クラスを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-137">You can use repository classes with or without a unit of work class.</span></span> <span data-ttu-id="d4fdd-138">すべてのエンティティ型、または種類ごとに 1 つの 1 つのリポジトリを実装することができます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-138">You can implement a single repository for all entity types, or one for each type.</span></span> <span data-ttu-id="d4fdd-139">種類ごとに 1 つを実装する場合は、別のクラス、ジェネリックの基底クラスと派生クラス、または抽象基本クラスおよび派生クラスを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-139">If you implement one for each type, you can use separate classes, a generic base class and derived classes, or an abstract base class and derived classes.</span></span> <span data-ttu-id="d4fdd-140">リポジトリにビジネス ロジックを含めるしたり、データ アクセス ロジックに制限できます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-140">You can include business logic in your repository or restrict it to data access logic.</span></span> <span data-ttu-id="d4fdd-141">使用して、データベース コンテキスト クラスに抽象化レイヤーを作成することも[した IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx)の代わりにあるインターフェイス[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)エンティティ セットの種類。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-141">You can also build an abstraction layer into your database context class by using [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) interfaces there instead of [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) types for your entity sets.</span></span> <span data-ttu-id="d4fdd-142">このチュートリアルで示すように抽象化レイヤーを実装する方法は、1 つのオプションを使用する、すべてのシナリオおよび環境の推奨設定されませんです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-142">The approach to implementing an abstraction layer shown in this tutorial is one option for you to consider, not a recommendation for all scenarios and environments.</span></span>


## <a name="creating-the-student-repository-class"></a><span data-ttu-id="d4fdd-143">学生リポジトリ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-143">Creating the Student Repository Class</span></span>

<span data-ttu-id="d4fdd-144">*DAL*フォルダー、という名前のクラス ファイルを作成する*IStudentRepository.cs*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-144">In the *DAL* folder, create a class file named *IStudentRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="d4fdd-145">このコードは、2 つの読み取りメソッドを含む、CRUD メソッドの標準的なセットを宣言 — すべてを返す`Student`エンティティ、および 1 つを検索する 1 つ`Student`ID によってエンティティ</span><span class="sxs-lookup"><span data-stu-id="d4fdd-145">This code declares a typical set of CRUD methods, including two read methods — one that returns all `Student` entities, and one that finds a single `Student` entity by ID.</span></span>

<span data-ttu-id="d4fdd-146">*DAL*フォルダー、という名前のクラス ファイルを作成する*StudentRepository.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-146">In the *DAL* folder, create a class file named *StudentRepository.cs* file.</span></span> <span data-ttu-id="d4fdd-147">実装する次のコードで、既存のコードを置き換えます、`IStudentRepository`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-147">Replace the existing code with the following code, which implements the `IStudentRepository` interface:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="d4fdd-148">クラスの変数で、データベース コンテキストが定義されているし、コンス トラクターは、コンテキストのインスタンスを渡すには、呼び出し元のオブジェクトが必要です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-148">The database context is defined in a class variable, and the constructor expects the calling object to pass in an instance of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="d4fdd-149">リポジトリに新しいコンテキストをインスタンス化する可能性がありますが、し、1 つのコント ローラー内の複数のリポジトリを使用した各しまいますは別のコンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-149">You could instantiate a new context in the repository, but then if you used multiple repositories in one controller, each would end up with a separate context.</span></span> <span data-ttu-id="d4fdd-150">内の複数のリポジトリを使用してを後で、`Course`コント ローラー、およびする作業クラスの単体ようにする方法のすべてのリポジトリが、同じコンテキストを使用することが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-150">Later you'll use multiple repositories in the `Course` controller, and you'll see how a unit of work class can ensure that all repositories use the same context.</span></span>

<span data-ttu-id="d4fdd-151">リポジトリを実装する[IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)コント ローラーは、前述のように、CRUD メソッドでは、データベース コンテキストに呼び出しを行う先ほど見たのと同じ方法では、データベース コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-151">The repository implements [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) and disposes the database context as you saw earlier in the controller, and its CRUD methods make calls to the database context in the same way that you saw earlier.</span></span>

## <a name="change-the-student-controller-to-use-the-repository"></a><span data-ttu-id="d4fdd-152">リポジトリを使用する、受講者コント ローラーの変更</span><span class="sxs-lookup"><span data-stu-id="d4fdd-152">Change the Student Controller to Use the Repository</span></span>

<span data-ttu-id="d4fdd-153">*StudentController.cs*クラスの現在のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-153">In *StudentController.cs*, replace the code currently in the class with the following code.</span></span> <span data-ttu-id="d4fdd-154">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-154">The changes are highlighted.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

<span data-ttu-id="d4fdd-155">コント ローラーは今すぐを実装するオブジェクトのクラス変数を宣言、`IStudentRepository`コンテキスト クラスの代わりにインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-155">The controller now declares a class variable for an object that implements the `IStudentRepository` interface instead of the context class:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="d4fdd-156">既定 (パラメーターなし) コンス トラクターが、新しいコンテキストのインスタンスを作成し、省略可能なコンス トラクターでは、呼び出し元のコンテキストのインスタンスを渡します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-156">The default (parameterless) constructor creates a new context instance, and an optional constructor allows the caller to pass in a context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="d4fdd-157">(を使用していた場合*依存性の注入*、または DI、DI ソフトウェアは、適切なリポジトリ オブジェクト名は常に提供されることを確認してくださいため既定のコンス トラクターを必要はありません。)。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-157">(If you were using *dependency injection*, or DI, you wouldn't need the default constructor because the DI software would ensure that the correct repository object would always be provided.)</span></span>

<span data-ttu-id="d4fdd-158">CRUD メソッドでコンテキストの代わりに、リポジトリと呼ばれるようになりました。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-158">In the CRUD methods, the repository is now called instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="d4fdd-159">および`Dispose`メソッドでは、コンテキストの代わりにリポジトリを破棄します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-159">And the `Dispose` method now disposes the repository instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="d4fdd-160">サイトを実行し、クリックして、**受講者**タブです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-160">Run the site and click the **Students** tab.</span></span>

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="d4fdd-162">ページは、検索し、同様に、リポジトリを使用するコードを変更して、他のページの受講者も同様に動作する前に、同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-162">The page looks and works the same as it did before you changed the code to use the repository, and the other Student pages also work the same.</span></span> <span data-ttu-id="d4fdd-163">ただし、方法で重要な違いがある、`Index`コント ローラーのメソッドはフィルター処理と順序付けします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-163">However, there's an important difference in the way the `Index` method of the controller does filtering and ordering.</span></span> <span data-ttu-id="d4fdd-164">このメソッドの元のバージョンには、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-164">The original version of this method contained the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

<span data-ttu-id="d4fdd-165">更新された`Index`メソッドには、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-165">The updated `Index` method contains the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

<span data-ttu-id="d4fdd-166">強調表示されたコードだけが変更されました。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-166">Only the highlighted code has changed.</span></span>

<span data-ttu-id="d4fdd-167">コードの元のバージョンで`students`として型指定されて、`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-167">In the original version of the code, `students` is typed as an `IQueryable` object.</span></span> <span data-ttu-id="d4fdd-168">などのメソッドを使用してコレクションに変換するまで、クエリがデータベースに送信されていない`ToList`、インデックス ビューにアクセスする、受講者モデルまで発生しません。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-168">The query isn't sent to the database until it's converted into a collection using a method such as `ToList`, which doesn't occur until the Index view accesses the student model.</span></span> <span data-ttu-id="d4fdd-169">`Where`上の元のコードでメソッドになります、`WHERE`データベースに送信される SQL クエリ内の句。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-169">The `Where` method in the original code above becomes a `WHERE` clause in the SQL query that is sent to the database.</span></span> <span data-ttu-id="d4fdd-170">さらに、選択したエンティティだけがデータベースによって返されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-170">That in turn means that only the selected entities are returned by the database.</span></span> <span data-ttu-id="d4fdd-171">変更した結果として、ただし、`context.Students`に`studentRepository.GetStudents()`、`students`後、このステートメントは、変数、`IEnumerable`データベース内のすべての受講者を含むコレクション。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-171">However, as a result of changing `context.Students` to `studentRepository.GetStudents()`, the `students` variable after this statement is an `IEnumerable` collection that includes all students in the database.</span></span> <span data-ttu-id="d4fdd-172">適用する際の最終結果、`Where`メソッドは同じですが、およびデータベースではなく、web サーバー上のメモリ内で作業を実行するようになりました。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-172">The end result of applying the `Where` method is the same, but now the work is done in memory on the web server and not by the database.</span></span> <span data-ttu-id="d4fdd-173">大量のデータを返すクエリでこのできます効率的です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-173">For queries that return large volumes of data, this can be inefficient.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="d4fdd-174">**Vs の IQueryable。IEnumerable**</span><span class="sxs-lookup"><span data-stu-id="d4fdd-174">**IQueryable vs. IEnumerable**</span></span>
> 
> <span data-ttu-id="d4fdd-175">実装した後、リポジトリ、ここで示すように値を入力する場合でも、**検索**ボックス SQL Server に送信されるクエリが、検索条件が含まれていないので、すべての学生行を返します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-175">After you implement the repository as shown here, even if you enter something in the **Search** box the query sent to SQL Server returns all Student rows because it doesn't include your search criteria:</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> <span data-ttu-id="d4fdd-176">このクエリは、リポジトリの検索条件に関するがわからなくても、クエリを実行するためのすべての学生データを返します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-176">This query returns all of the student data because the repository executed the query without knowing about the search criteria.</span></span> <span data-ttu-id="d4fdd-177">プロセスの並べ替え、検索条件を適用すること、およびページング (ここでは 3 個の行が表示される) がメモリ内で実行のデータのサブセットを選択するときに後で、`ToPagedList`でメソッドが呼び出さ、`IEnumerable`コレクション。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-177">The process of sorting, applying search criteria, and selecting a subset of the data for paging (showing only 3 rows in this case) is done in memory later when the `ToPagedList` method is called on the `IEnumerable` collection.</span></span>
> 
> <span data-ttu-id="d4fdd-178">(リポジトリを実装して) する前に、コードの以前のバージョンで、クエリがデータベースに送信されません、まで、検索条件を適用した後と`ToPagedList`で呼び出されると、`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-178">In the previous version of the code (before you implemented the repository), the query is not sent to the database until after you apply the search criteria, when `ToPagedList` is called on the `IQueryable` object.</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> <span data-ttu-id="d4fdd-179">ToPagedList が呼び出されたとき、`IQueryable`オブジェクト、SQL Server に送信されるクエリ、検索文字列を指定し、その結果を検索条件を満たす行だけが返されます、およびフィルター処理する必要はありませんメモリ内で実行します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-179">When ToPagedList is called on an `IQueryable` object, the query sent to SQL Server specifies the search string, and as a result only rows that meet the search criteria are returned, and no filtering needs to be done in memory.</span></span>
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> <span data-ttu-id="d4fdd-180">(以下のチュートリアルは、SQL Server に送信されるクエリを確認する方法を説明します)。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-180">(The following tutorial explains how to examine queries sent to SQL Server.)</span></span>


<span data-ttu-id="d4fdd-181">次のセクションでは、データベースでこの作業を行う必要がありますを指定できるリポジトリ メソッドを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-181">The following section shows how to implement repository methods that enable you to specify that this work should be done by the database.</span></span>

<span data-ttu-id="d4fdd-182">コント ローラーと、Entity Framework データベースのコンテキストの抽象化レイヤーが作成されました。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-182">You've now created an abstraction layer between the controller and the Entity Framework database context.</span></span> <span data-ttu-id="d4fdd-183">実装する単体テスト プロジェクトでリポジトリの代替クラスを作成する自動化された単体テストでこのアプリケーションを実行する場合は、でした`IStudentRepository`*です。*</span><span class="sxs-lookup"><span data-stu-id="d4fdd-183">If you were going to perform automated unit testing with this application, you could create an alternative repository class in a unit test project that implements `IStudentRepository`*.*</span></span> <span data-ttu-id="d4fdd-184">データを読み書きするコンテキストを呼び出す代わりにこのモック リポジトリ クラスは、コント ローラーの機能をテストするためにメモリ内コレクションを操作する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-184">Instead of calling the context to read and write data, this mock repository class could manipulate in-memory collections in order to test controller functions.</span></span>

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a><span data-ttu-id="d4fdd-185">汎用的なリポジトリと作業クラスの単位を実装します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-185">Implement a Generic Repository and a Unit of Work Class</span></span>

<span data-ttu-id="d4fdd-186">多くの冗長なコードは、各エンティティ型に対してリポジトリ クラスを作成することがあり、部分的な更新プログラムを可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-186">Creating a repository class for each entity type could result in a lot of redundant code, and it could result in partial updates.</span></span> <span data-ttu-id="d4fdd-187">たとえば、同じトランザクションの一部として 2 つの別のエンティティ型を更新する必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-187">For example, suppose you have to update two different entity types as part of the same transaction.</span></span> <span data-ttu-id="d4fdd-188">それぞれには、別のデータベース コンテキストのインスタンスが使用されている場合は、成功いずれかの可能性があります、もう一方が失敗します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-188">If each uses a separate database context instance, one might succeed and the other might fail.</span></span> <span data-ttu-id="d4fdd-189">重複するコードを最小限に抑える方法の 1 つは、汎用的なリポジトリとことを確認する方法の 1 つを使用するすべてのリポジトリが同じデータベース コンテキストを使用して (およびためすべての更新プログラムを調整) は作業クラスの単位を使用します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-189">One way to minimize redundant code is to use a generic repository, and one way to ensure that all repositories use the same database context (and thus coordinate all updates) is to use a unit of work class.</span></span>

<span data-ttu-id="d4fdd-190">チュートリアルのこのセクションで作成、`GenericRepository`クラスおよび`UnitOfWork`クラス、およびそれらを使用して、`Course`両方にアクセスするコント ローラー、`Department`と`Course`エンティティ セット。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-190">In this section of the tutorial, you'll create a `GenericRepository` class and a `UnitOfWork` class, and use them in the `Course` controller to access both the `Department` and the `Course` entity sets.</span></span> <span data-ttu-id="d4fdd-191">前述のように、チュートリアルのこの部分をシンプルに作成するには、これらのクラス インターフェイス</span><span class="sxs-lookup"><span data-stu-id="d4fdd-191">As explained earlier, to keep this part of the tutorial simple, you aren't creating interfaces for these classes.</span></span> <span data-ttu-id="d4fdd-192">TDD を容易にするために使用された場合は、通常実装するそれらのインターフェイスで実行したのと同様ですが、`Student`リポジトリです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-192">But if you were going to use them to facilitate TDD, you'd typically implement them with interfaces the same way you did the `Student` repository.</span></span>

### <a name="create-a-generic-repository"></a><span data-ttu-id="d4fdd-193">汎用的なリポジトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-193">Create a Generic Repository</span></span>

<span data-ttu-id="d4fdd-194">*DAL*フォルダー作成*GenericRepository.cs*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-194">In the *DAL* folder, create *GenericRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

<span data-ttu-id="d4fdd-195">クラスの変数は、データベースのコンテキストとのリポジトリがインスタンス化されるエンティティ セットに対して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-195">Class variables are declared for the database context and for the entity set that the repository is instantiated for:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="d4fdd-196">コンス トラクターは、データベース コンテキストのインスタンスを受け取り、エンティティ セットの変数を初期化します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-196">The constructor accepts a database context instance and initializes the entity set variable:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="d4fdd-197">`Get`メソッドでは、ラムダ式を使用して、フィルター条件と、結果の並べ替えに列を指定する呼び出し元のコードを許可して、文字列パラメーターにより、呼び出し元は、一括読み込みのナビゲーション プロパティのコンマ区切りの一覧を提供します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-197">The `Get` method uses lambda expressions to allow the calling code to specify a filter condition and a column to order the results by, and a string parameter lets the caller provide a comma-delimited list of navigation properties for eager loading:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="d4fdd-198">コード`Expression<Func<TEntity, bool>> filter`、呼び出し元は、ラムダ式に基づいて、なります、`TEntity`型、およびこの式はブール値を返します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-198">The code `Expression<Func<TEntity, bool>> filter` means the caller will provide a lambda expression based on the `TEntity` type, and this expression will return a Boolean value.</span></span> <span data-ttu-id="d4fdd-199">リポジトリがのインスタンス化される場合など、`Student`エンティティ型、呼び出し元のメソッドのコードを指定`student => student.LastName == "Smith`&quot;の`filter`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-199">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `student => student.LastName == "Smith`&quot; for the `filter` parameter.</span></span>

<span data-ttu-id="d4fdd-200">コード`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`も呼び出し元がラムダ式を渡すことを意味します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-200">The code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` also means the caller will provide a lambda expression.</span></span> <span data-ttu-id="d4fdd-201">この場合、式への入力、`IQueryable`オブジェクトに対して、`TEntity`型です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-201">But in this case, the input to the expression is an `IQueryable` object for the `TEntity` type.</span></span> <span data-ttu-id="d4fdd-202">式は順序付けられたバージョンを返します`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-202">The expression will return an ordered version of that `IQueryable` object.</span></span> <span data-ttu-id="d4fdd-203">リポジトリがのインスタンス化される場合など、`Student`エンティティ型、呼び出し元のメソッドのコードを指定`q => q.OrderBy(s => s.LastName)`の`orderBy`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-203">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `q => q.OrderBy(s => s.LastName)` for the `orderBy` parameter.</span></span>

<span data-ttu-id="d4fdd-204">内のコード、`Get`メソッドを作成、`IQueryable`オブジェクトを 1 つを使用する必要がある場合、フィルター式を適用します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-204">The code in the `Get` method creates an `IQueryable` object and then applies the filter expression if there is one:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="d4fdd-205">次に、コンマ区切りの一覧を解析した後は、一括読み込みの式が適用されます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-205">Next it applies the eager-loading expressions after parsing the comma-delimited list:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

<span data-ttu-id="d4fdd-206">最後に、適用される、`orderBy`式のいずれかを使用する必要がある場合です。 その結果を返しますそれ以外の場合、順序付けられていないクエリから結果を返します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-206">Finally, it applies the `orderBy` expression if there is one and returns the results; otherwise it returns the results from the unordered query:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

<span data-ttu-id="d4fdd-207">呼び出すと、`Get`メソッド、適用するにはフィルター処理と並べ替え、`IEnumerable`これらの関数のパラメーターを提供する代わりに、メソッドによって返されるコレクション。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-207">When you call the `Get` method, you could do filtering and sorting on the `IEnumerable` collection returned by the method instead of providing parameters for these functions.</span></span> <span data-ttu-id="d4fdd-208">Web サーバー上のメモリに並べ替えと作業をフィルター処理を完了できるとします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-208">But the sorting and filtering work would then be done in memory on the web server.</span></span> <span data-ttu-id="d4fdd-209">これらのパラメーターを使用すると、web サーバーではなく、データベースでの処理を実行することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-209">By using these parameters, you ensure that the work is done by the database rather than the web server.</span></span> <span data-ttu-id="d4fdd-210">代わりに、特定のエンティティ型の派生クラスを作成し、特殊な追加`Get`メソッドなど`GetStudentsInNameOrder`または`GetStudentsByName`です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-210">An alternative is to create derived classes for specific entity types and add specialized `Get` methods, such as `GetStudentsInNameOrder` or `GetStudentsByName`.</span></span> <span data-ttu-id="d4fdd-211">ただし、複雑なアプリケーションでは、これが生じることが非常に多数のような派生クラスと特殊なメソッドより多くの作業を維持する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-211">However, in a complex application, this can result in a large number of such derived classes and specialized methods, which could be more work to maintain.</span></span>

<span data-ttu-id="d4fdd-212">内のコード、 `GetByID`、 `Insert`、および`Update`メソッドは、非ジェネリック リポジトリで学習した内容に似ています。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-212">The code in the `GetByID`, `Insert`, and `Update` methods is similar to what you saw in the non-generic repository.</span></span> <span data-ttu-id="d4fdd-213">(の一括読み込みパラメーターを提供することは、`GetByID`署名で一括読み込みを行うことはできませんので、`Find`メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-213">(You aren't providing an eager loading parameter in the `GetByID` signature, because you can't do eager loading with the `Find` method.)</span></span>

<span data-ttu-id="d4fdd-214">2 つのオーバー ロードに提供される、`Delete`メソッド。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-214">Two overloads are provided for the `Delete` method:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

<span data-ttu-id="d4fdd-215">削除するエンティティの ID のみによりこれらのいずれかを渡すし、エンティティ インスタンスを受け取り、1 つです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-215">One of these lets you pass in just the ID of the entity to be deleted, and one takes an entity instance.</span></span> <span data-ttu-id="d4fdd-216">説明したとおり、[同時実行の処理](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)同時処理する必要があるは、チュートリアル、`Delete`をエンティティのインスタンスを受け取るメソッドには、追跡プロパティの元の値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-216">As you saw in the [Handling Concurrency](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial, for concurrency handling you need a `Delete` method that takes an entity instance that includes the original value of a tracking property.</span></span>

<span data-ttu-id="d4fdd-217">この汎用的なリポジトリでは、一般的な CRUD 要件を処理します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-217">This generic repository will handle typical CRUD requirements.</span></span> <span data-ttu-id="d4fdd-218">特定のエンティティ型により複雑なフィルター処理や、順序付けなどの特別な要件がある場合は、その種類の追加方法を持つ派生クラスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-218">When a particular entity type has special requirements, such as more complex filtering or ordering, you can create a derived class that has additional methods for that type.</span></span>

## <a name="creating-the-unit-of-work-class"></a><span data-ttu-id="d4fdd-219">クラスの作業の単位を作成します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-219">Creating the Unit of Work Class</span></span>

<span data-ttu-id="d4fdd-220">クラスの作業の単位は 1 つの目的を果たします。 ように複数のリポジトリを使用すると、1 つのデータベース コンテキストを共有します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-220">The unit of work class serves one purpose: to make sure that when you use multiple repositories, they share a single database context.</span></span> <span data-ttu-id="d4fdd-221">こうすれば、作業の単位が完了すると呼び出すことができます、`SaveChanges`メソッド コンテキストのインスタンスを関連するすべての変更が連携することが確実に実行するとします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-221">That way, when a unit of work is complete you can call the `SaveChanges` method on that instance of the context and be assured that all related changes will be coordinated.</span></span> <span data-ttu-id="d4fdd-222">クラスの必要なものはすべて、`Save`メソッドおよび各リポジトリのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-222">All that the class needs is a `Save` method and a property for each repository.</span></span> <span data-ttu-id="d4fdd-223">リポジトリの各プロパティは、他のリポジトリ インスタンスと同じデータベース コンテキストのインスタンスを使用してインスタンス化されているリポジトリのインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-223">Each repository property returns a repository instance that has been instantiated using the same database context instance as the other repository instances.</span></span>

<span data-ttu-id="d4fdd-224">*DAL*フォルダー、という名前のクラス ファイルを作成する*UnitOfWork.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-224">In the *DAL* folder, create a class file named *UnitOfWork.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="d4fdd-225">コードでは、データベースのコンテキストと各リポジトリ用のクラス変数を作成します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-225">The code creates class variables for the database context and each repository.</span></span> <span data-ttu-id="d4fdd-226">`context`変数、新しいコンテキストがインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-226">For the `context` variable, a new context is instantiated:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="d4fdd-227">リポジトリの各プロパティは、リポジトリが既に存在するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-227">Each repository property checks whether the repository already exists.</span></span> <span data-ttu-id="d4fdd-228">それ以外の場合は、コンテキストのインスタンスを渡して、リポジトリをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-228">If not, it instantiates the repository, passing in the context instance.</span></span> <span data-ttu-id="d4fdd-229">その結果、すべてのリポジトリは、同じコンテキスト インスタンスを共有します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-229">As a result, all repositories share the same context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="d4fdd-230">`Save`メソッド呼び出し`SaveChanges`データベース コンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-230">The `Save` method calls `SaveChanges` on the database context.</span></span>

<span data-ttu-id="d4fdd-231">クラスの変数で、データベース コンテキストをインスタンス化するすべてのクラスと同様に、`UnitOfWork`クラスが実装する`IDisposable`コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-231">Like any class that instantiates a database context in a class variable, the `UnitOfWork` class implements `IDisposable` and disposes the context.</span></span>

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a><span data-ttu-id="d4fdd-232">UnitOfWork クラスとリポジトリを使用するコース コント ローラーを変更します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-232">Changing the Course Controller to use the UnitOfWork Class and Repositories</span></span>

<span data-ttu-id="d4fdd-233">現在のあるコードを置き換える*CourseController.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-233">Replace the code you currently have in *CourseController.cs* with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

<span data-ttu-id="d4fdd-234">このコードがクラスに変数を追加、`UnitOfWork`クラスです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-234">This code adds a class variable for the `UnitOfWork` class.</span></span> <span data-ttu-id="d4fdd-235">(ここで、インターフェイスを使用していた場合、は、ここに変数を初期化する考えないから以外場合と同様に 2 つのコンス トラクターのパターンを実装するが代わりの場合は、`Student`リポジトリです)。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-235">(If you were using interfaces here, you wouldn't initialize the variable here; instead, you'd implement a pattern of two constructors just as you did for the `Student` repository.)</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

<span data-ttu-id="d4fdd-236">クラスの残りの部分で、データベース コンテキストに対するすべての参照に置換された、適切なリポジトリへの参照を使用して`UnitOfWork`リポジトリにアクセスするプロパティです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-236">In the rest of the class, all references to the database context are replaced by references to the appropriate repository, using `UnitOfWork` properties to access the repository.</span></span> <span data-ttu-id="d4fdd-237">`Dispose`メソッドの破棄、`UnitOfWork`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-237">The `Dispose` method disposes the `UnitOfWork` instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

<span data-ttu-id="d4fdd-238">サイトを実行し、クリックして、**コース**タブです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-238">Run the site and click the **Courses** tab.</span></span>

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="d4fdd-240">ページは、検索し、変更内容は、以前と同じし、他のページのコースはまた、同じ動作と、同様に機能します。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-240">The page looks and works the same as it did before your changes, and the other Course pages also work the same.</span></span>

## <a name="summary"></a><span data-ttu-id="d4fdd-241">まとめ</span><span class="sxs-lookup"><span data-stu-id="d4fdd-241">Summary</span></span>

<span data-ttu-id="d4fdd-242">リポジトリと作業パターンの単位の両方を実装しているようになりました。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-242">You have now implemented both the repository and unit of work patterns.</span></span> <span data-ttu-id="d4fdd-243">ジェネリックのリポジトリ内のメソッド パラメーターとしてラムダ式を使用しています。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-243">You have used lambda expressions as method parameters in the generic repository.</span></span> <span data-ttu-id="d4fdd-244">これらの式を使用する方法の詳細についての`IQueryable`オブジェクトを参照してください[IQueryable(T) インターフェイス (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-244">For more information about how to use these expressions with an `IQueryable` object, see [IQueryable(T) Interface (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) in the MSDN Library.</span></span> <span data-ttu-id="d4fdd-245">次のチュートリアルの一部を処理する方法を学習には、シナリオが高度な。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-245">In the next tutorial you'll learn how to handle some advanced scenarios.</span></span>

<span data-ttu-id="d4fdd-246">その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="d4fdd-246">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d4fdd-247">[前へ](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="d4fdd-247">[Previous](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>