---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC 5 アプリケーション (11/12) で Entity Framework 6 による継承の実装 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 118233338112a71216b909b1dabed2333bfa235e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a><span data-ttu-id="25654-103">ASP.NET MVC 5 アプリケーション (11/12) で Entity Framework 6 による継承の実装</span><span class="sxs-lookup"><span data-stu-id="25654-103">Implementing Inheritance with the Entity Framework 6 in an ASP.NET MVC 5 Application (11 of 12)</span></span>
====================
<span data-ttu-id="25654-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="25654-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="25654-105">[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="25654-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="25654-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="25654-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="25654-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="25654-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="25654-108">前のチュートリアルでは、同時実行例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="25654-108">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="25654-109">このチュートリアルでは、データ モデルでの継承を実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="25654-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="25654-110">オブジェクト指向プログラミングで使用できます[継承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))を容易にする[コードの再利用](http://en.wikipedia.org/wiki/Code_reuse)です。</span><span class="sxs-lookup"><span data-stu-id="25654-110">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="25654-111">このチュートリアルでは、変更、`Instructor`と`Student`クラスから派生するように、`Person`基底クラスなどのプロパティを含む`LastName`講習においてインストラクターと受講者の両方に共通であります。</span><span class="sxs-lookup"><span data-stu-id="25654-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="25654-112">されませんを追加または web ページは変更は、コードの一部を変更しますが、し、それらの変更がデータベースに自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="25654-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="25654-113">継承をデータベース テーブルにマップするためのオプション</span><span class="sxs-lookup"><span data-stu-id="25654-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="25654-114">`Instructor`と`Student`内のクラス、`School`データ モデルと同じではいくつかのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="25654-114">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="25654-116">共有されるプロパティを冗長なコードを削除すると、`Instructor`と`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="25654-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="25654-117">または、インストラクターまたは受講者から名前が付属しているかどうかをかけることがなく名を書式設定できるサービスを記述します。</span><span class="sxs-lookup"><span data-stu-id="25654-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="25654-118">作成することが、`Person`プロパティ、その共有だけが含まれているクラスを基本にしてください、`Instructor`と`Student`エンティティは、次の図に示すように、その基本クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="25654-118">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="25654-120">これには、データベースでこの継承構造を表すことができますいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="25654-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="25654-121">割り当てることも、`Person`受講者と講習においてインストラクターに 1 つのテーブルの両方に関する情報を含むテーブル。</span><span class="sxs-lookup"><span data-stu-id="25654-121">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="25654-122">講習においてインストラクターにのみ適用できなかった一部の列 (`HireDate`)、受講者にのみ一部 (`EnrollmentDate`)、一部には、両方 (`LastName`、 `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="25654-122">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="25654-123">通常、する必要があります、*識別子*各行を入力するかを示す列を表します。</span><span class="sxs-lookup"><span data-stu-id="25654-123">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="25654-124">たとえば、識別子の列は、受講者のインストラクターと「生徒」「インストラクター」があります。</span><span class="sxs-lookup"><span data-stu-id="25654-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="25654-126">1 つのデータベース テーブルからエンティティ継承構造を生成するには、このパターンが呼び出された*テーブルの階層あたり*(TPH) 継承します。</span><span class="sxs-lookup"><span data-stu-id="25654-126">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="25654-127">代わりに、継承構造と同じように、データベースの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="25654-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="25654-128">たとえば name フィールドのみを持つことが、`Person`テーブルし、は独立した`Instructor`と`Student`日付フィールドを持つテーブルです。</span><span class="sxs-lookup"><span data-stu-id="25654-128">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="25654-130">各エンティティ クラスと呼ばれるデータベース テーブルのこのパターン*型ごとにテーブル*(TPT) 継承します。</span><span class="sxs-lookup"><span data-stu-id="25654-130">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="25654-131">まだ他のオプションは、個々 のテーブルにすべての非抽象型をマップするです。</span><span class="sxs-lookup"><span data-stu-id="25654-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="25654-132">継承されたプロパティを含むクラスのすべてのプロパティは、対応するテーブルの列にマップします。</span><span class="sxs-lookup"><span data-stu-id="25654-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="25654-133">このパターンは、具象型でのテーブルごとのクラス (TPC) の継承と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="25654-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="25654-134">継承を TPC を実装している場合、 `Person`、 `Student`、および`Instructor`、前に示したようにクラス、`Student`と`Instructor`テーブルはよりも長く前に、継承を実装した後にまったく同じなります。</span><span class="sxs-lookup"><span data-stu-id="25654-134">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="25654-135">TPC と TPH 継承パターン TPT 継承のパターンよりも、Entity Framework のパフォーマンスを向上させるが通常提供 TPT パターンが複雑な結合クエリのために使用します。</span><span class="sxs-lookup"><span data-stu-id="25654-135">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="25654-136">このチュートリアルでは、TPH 継承の実装方法を示します。</span><span class="sxs-lookup"><span data-stu-id="25654-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="25654-137">TPH は Entity Framework での既定の継承パターンを作成では行う必要があるすべて、`Person`クラス、変更、`Instructor`と`Student`クラスから派生する`Person`、新しいクラスを追加、 `DbContext`、作成し、移行します。</span><span class="sxs-lookup"><span data-stu-id="25654-137">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="25654-138">(その他の継承パターンを実装する方法については、次を参照してください[マッピング テーブルの種類ごと (TPT) 継承](https://msdn.microsoft.com/data/jj591617#2.5)と[具象型でのテーブルあたりクラス (TPC) の継承をマッピング](https://msdn.microsoft.com/data/jj591617#2.6)、MSDN の。Entity Framework ドキュメントです。)</span><span class="sxs-lookup"><span data-stu-id="25654-138">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="25654-139">ユーザー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="25654-139">Create the Person class</span></span>

<span data-ttu-id="25654-140">*モデル*フォルダー作成*Person.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="25654-140">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="25654-141">ユーザーから継承 Student とインストラクター クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="25654-141">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="25654-142">*Instructor.cs*、派生、`Instructor`クラス、`Person`クラスし、キーと名前のフィールドを削除します。</span><span class="sxs-lookup"><span data-stu-id="25654-142">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="25654-143">コードは、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="25654-143">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="25654-144">同様に変更を*Student.cs*です。</span><span class="sxs-lookup"><span data-stu-id="25654-144">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="25654-145">`Student`クラスは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="25654-145">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a><span data-ttu-id="25654-146">Person エンティティ型をモデルに追加します。</span><span class="sxs-lookup"><span data-stu-id="25654-146">Add the Person Entity Type to the Model</span></span>

<span data-ttu-id="25654-147">*SchoolContext.cs*、追加、`DbSet`プロパティを`Person`エンティティの種類。</span><span class="sxs-lookup"><span data-stu-id="25654-147">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="25654-148">これは、Entity Framework は、テーブルの階層あたりの継承を構成するために必要なです。</span><span class="sxs-lookup"><span data-stu-id="25654-148">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="25654-149">わかる、データベースが更新されたとき、`Person`の代わりにテーブル、`Student`と`Instructor`テーブル。</span><span class="sxs-lookup"><span data-stu-id="25654-149">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="25654-150">作成し、移行ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="25654-150">Create and Update a Migrations File</span></span>

<span data-ttu-id="25654-151">パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="25654-151">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="25654-152">実行、 `Update-Database` PMC コマンド。</span><span class="sxs-lookup"><span data-stu-id="25654-152">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="25654-153">コマンドは、既存のデータ移行を処理する方法を知らないにしているために、この時点で失敗します。</span><span class="sxs-lookup"><span data-stu-id="25654-153">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="25654-154">次のようなエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="25654-154">You get an error message like the following one:</span></span>

> <span data-ttu-id="25654-155">*オブジェクトを削除できませんでした ' dbo します。講師 ' FOREIGN KEY 制約で参照されているためです。*</span><span class="sxs-lookup"><span data-stu-id="25654-155">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="25654-156">開いている*移行\&lt; タイムスタンプ&gt;\_Inheritance.cs*と置換、`Up`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="25654-156">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="25654-157">このコードは、次のデータベースの更新タスクの処理します。</span><span class="sxs-lookup"><span data-stu-id="25654-157">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="25654-158">外部キー制約と Student テーブルをポイントするインデックスを削除します。</span><span class="sxs-lookup"><span data-stu-id="25654-158">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="25654-159">個人として教師テーブルの名前を変更し、受講者用のデータを格納するために必要な変更を行います。</span><span class="sxs-lookup"><span data-stu-id="25654-159">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="25654-160">受講者の null 許容 EnrollmentDate を追加します。</span><span class="sxs-lookup"><span data-stu-id="25654-160">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="25654-161">行が、受講者または講師がかどうかを示すために識別子列を追加します。</span><span class="sxs-lookup"><span data-stu-id="25654-161">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="25654-162">により HireDate null 許容ため学生行は、雇用日を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="25654-162">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="25654-163">受講者をポイントする外部キーの更新に使用される一時的なフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="25654-163">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="25654-164">Person テーブルに受講者をコピーするときに、新しい主キー値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25654-164">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="25654-165">Person テーブルに、Student テーブルからデータをコピーします。</span><span class="sxs-lookup"><span data-stu-id="25654-165">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="25654-166">これにより、受講者が割り当てられている新しい主キー値を取得します。</span><span class="sxs-lookup"><span data-stu-id="25654-166">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="25654-167">受講者をポイントする外部キー値を修正します。</span><span class="sxs-lookup"><span data-stu-id="25654-167">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="25654-168">外部キー制約と今すぐ Person テーブルをポイントして、インデックスを再作成されます。</span><span class="sxs-lookup"><span data-stu-id="25654-168">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="25654-169">(学生主キーの値を変更する必要はない場合は、プライマリ キーの種類として、整数ではなく GUID を使用した、および省略されていることができいくつかの手順です。)</span><span class="sxs-lookup"><span data-stu-id="25654-169">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="25654-170">実行、`update-database`もう一度コマンドします。</span><span class="sxs-lookup"><span data-stu-id="25654-170">Run the `update-database` command again.</span></span>

<span data-ttu-id="25654-171">(実稼働システムでは対応に変更を加えるダウン メソッド場合に、データベースの以前のバージョンに戻るに使用しなければならなかったことです。</span><span class="sxs-lookup"><span data-stu-id="25654-171">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="25654-172">このチュートリアルではありませんする方法を使用してダウンします。)</span><span class="sxs-lookup"><span data-stu-id="25654-172">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="25654-173">データとスキーマの変更を移行するときにその他のエラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="25654-173">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="25654-174">移行エラーが発生する場合は解決できない場合、内の接続文字列を変更することで、チュートリアルを続行することができます、 *Web.config*ファイルまたはでデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="25654-174">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="25654-175">最も簡単な方法が内のデータベースの名前を変更するには、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="25654-175">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="25654-176">たとえば、次の例で示すように ContosoUniversity2 にデータベース名を変更します。</span><span class="sxs-lookup"><span data-stu-id="25654-176">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> <span data-ttu-id="25654-177">新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="25654-177">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="25654-178">データベースを削除する方法については、次を参照してください。[を Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)です。</span><span class="sxs-lookup"><span data-stu-id="25654-178">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="25654-179">チュートリアルを続行するためにこの方法を採用する場合、このチュートリアルの最後の配置手順をスキップまたは新しいサイトとデータベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="25654-179">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="25654-180">したされてに配置する、既に同じサイトに更新プログラムを展開する場合 EF で移行を自動的に実行するときに同じエラーがありますが表示されます。</span><span class="sxs-lookup"><span data-stu-id="25654-180">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="25654-181">移行エラーのトラブルシューティングする場合に最適なリソースは Entity Framework のフォーラムや StackOverflow.com のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="25654-181">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="25654-182">テスト中</span><span class="sxs-lookup"><span data-stu-id="25654-182">Testing</span></span>

<span data-ttu-id="25654-183">サイトを実行して、さまざまなページを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="25654-183">Run the site and try various pages.</span></span> <span data-ttu-id="25654-184">前に、と同じに動作します。</span><span class="sxs-lookup"><span data-stu-id="25654-184">Everything works the same as it did before.</span></span>

<span data-ttu-id="25654-185">**サーバー エクスプ ローラーで、**展開**データ Connections\SchoolContext**し**テーブル**、されたことを確認し、**学生**と**インストラクター**テーブルに置換された、 **Person**テーブル。</span><span class="sxs-lookup"><span data-stu-id="25654-185">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="25654-186">展開、 **Person**テーブルとしているときに存在するために使用される列のすべて、**学生**と**インストラクター**テーブル。</span><span class="sxs-lookup"><span data-stu-id="25654-186">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="25654-188">Person テーブルを右クリックし、をクリックして**テーブル データの表示**識別子列を表示します。</span><span class="sxs-lookup"><span data-stu-id="25654-188">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="25654-189">次の図は、新しい School データベースの構造を示しています。</span><span class="sxs-lookup"><span data-stu-id="25654-189">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="25654-191">Azure への配置します。</span><span class="sxs-lookup"><span data-stu-id="25654-191">Deploy to Azure</span></span>

<span data-ttu-id="25654-192">このセクションでは、省略可能な完了している必要があります**Azure へのアプリの展開**」の「[パート 3、並べ替え、フィルター、およびページング](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)このチュートリアル シリーズのです。</span><span class="sxs-lookup"><span data-stu-id="25654-192">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="25654-193">ローカル プロジェクトでデータベースを削除することによって解決移行エラーがある場合です。 この手順をスキップします。または、新しいサイトとデータベースを作成し、新しい環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="25654-193">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="25654-194">Visual Studio でプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**コンテキスト メニューからです。</span><span class="sxs-lookup"><span data-stu-id="25654-194">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>  
  
    ![プロジェクトのコンテキスト メニューを発行します。](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. <span data-ttu-id="25654-196">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25654-196">Click **Publish**.</span></span>  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
 <span data-ttu-id="25654-198">Web アプリは、既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="25654-198">The Web app will open in your default browser.</span></span>
3. <span data-ttu-id="25654-199">アプリケーションをテストすることを確認して動作します。</span><span class="sxs-lookup"><span data-stu-id="25654-199">Test the application to verify it's working.</span></span>

    <span data-ttu-id="25654-200">初めてページを実行するデータベースにアクセスする場合は、Entity Framework 実行のすべての移行の`Up`データベースが現在のデータ モデルに最新の状態に必要なメソッドです。</span><span class="sxs-lookup"><span data-stu-id="25654-200">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="summary"></a><span data-ttu-id="25654-201">まとめ</span><span class="sxs-lookup"><span data-stu-id="25654-201">Summary</span></span>

<span data-ttu-id="25654-202">テーブルの階層あたりの継承を実装したら、 `Person`、 `Student`、および`Instructor`クラスです。</span><span class="sxs-lookup"><span data-stu-id="25654-202">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="25654-203">このおよびその他の継承構造の詳細については、次を参照してください。 [TPT の継承パターン](https://msdn.microsoft.com/data/jj618293)と[TPH の継承パターン](https://msdn.microsoft.com/data/jj618292)msdn です。</span><span class="sxs-lookup"><span data-stu-id="25654-203">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="25654-204">次のチュートリアルでは、さまざまな Entity Framework の比較的高度なシナリオを処理する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25654-204">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

<span data-ttu-id="25654-205">その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="25654-205">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="25654-206">[前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="25654-206">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>