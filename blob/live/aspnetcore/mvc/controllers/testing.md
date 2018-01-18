---
title: "ASP.NET Core でのテスト コント ローラー ロジック"
author: ardalis
description: "ASP.NET Core Moq と xUnit でコント ローラーのロジックをテストする方法を説明します。"
keywords: "ASP.NET Core、コント ローラー、テスト、テスト、単体テスト、単体テスト、統合テスト、統合テスト、xUnit"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: dd4135ec-2b15-410c-b3fb-3d12eed4a1ac
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: aa60912e06946bd0df4936d33c88d3bf7b69984c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a><span data-ttu-id="fd82e-104">ASP.NET Core でのテスト コント ローラー ロジック</span><span class="sxs-lookup"><span data-stu-id="fd82e-104">Testing controller logic in ASP.NET Core</span></span>

<span data-ttu-id="fd82e-105">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fd82e-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fd82e-106">ASP.NET MVC アプリケーション内のコント ローラーは、ユーザー インターフェイスの問題に焦点を当てており、小規模にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd82e-106">Controllers in ASP.NET MVC apps should be small and focused on user-interface concerns.</span></span> <span data-ttu-id="fd82e-107">UI 以外の問題に対処する大規模なコント ローラーは、テストや保守が困難です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-107">Large controllers that deal with non-UI concerns are more difficult to test and maintain.</span></span>

[<span data-ttu-id="fd82e-108">GitHub から表示またはダウンロードのサンプル</span><span class="sxs-lookup"><span data-stu-id="fd82e-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a><span data-ttu-id="fd82e-109">テスト コント ローラー</span><span class="sxs-lookup"><span data-stu-id="fd82e-109">Testing controllers</span></span>

<span data-ttu-id="fd82e-110">コント ローラーは、すべての ASP.NET Core MVC アプリケーションの中央部分です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-110">Controllers are a central part of any ASP.NET Core MVC application.</span></span> <span data-ttu-id="fd82e-111">そのため、アプリが意図したとおりに動作する信頼が必要です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-111">As such, you should have confidence they behave as intended for your app.</span></span> <span data-ttu-id="fd82e-112">自動テストは、この信頼を提供でき、実稼働環境に到達する前に、エラーを検出できます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-112">Automated tests can provide you with this confidence and can detect errors before they reach production.</span></span> <span data-ttu-id="fd82e-113">ことが重要、コント ローラー内で不要な責任を配置することを回避し、テストのみに集中コント ローラーの機能を確認してください。</span><span class="sxs-lookup"><span data-stu-id="fd82e-113">It's important to avoid placing unnecessary responsibilities within your controllers and ensure your tests focus only on controller responsibilities.</span></span>

<span data-ttu-id="fd82e-114">コント ローラー ロジックは最小限にする必要があり、ビジネス ロジックまたはインフラストラクチャの問題 (たとえば、データ アクセス) に注目せずします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-114">Controller logic should be minimal and not be focused on business logic or infrastructure concerns (for example, data access).</span></span> <span data-ttu-id="fd82e-115">フレームワークではない、コント ローラー ロジックをテストします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-115">Test controller logic, not the framework.</span></span> <span data-ttu-id="fd82e-116">テスト方法、コント ローラー*動作*有効または無効な入力に基づきます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-116">Test how the controller *behaves* based on valid or invalid inputs.</span></span> <span data-ttu-id="fd82e-117">実行するビジネス操作の結果に基づくコント ローラーの応答をテストします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-117">Test controller responses based on the result of the business operation it performs.</span></span>

<span data-ttu-id="fd82e-118">一般的なコント ローラーの機能:</span><span class="sxs-lookup"><span data-stu-id="fd82e-118">Typical controller responsibilities:</span></span>

* <span data-ttu-id="fd82e-119">確認`ModelState.IsValid`です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-119">Verify `ModelState.IsValid`.</span></span>
* <span data-ttu-id="fd82e-120">場合、エラー応答を返す`ModelState`が無効です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-120">Return an error response if `ModelState` is invalid.</span></span>
* <span data-ttu-id="fd82e-121">ビジネス エンティティを永続化ストアから取得します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-121">Retrieve a business entity from persistence.</span></span>
* <span data-ttu-id="fd82e-122">ビジネス エンティティに対して操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-122">Perform an action on the business entity.</span></span>
* <span data-ttu-id="fd82e-123">永続化されたビジネス エンティティを保存します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-123">Save the business entity to persistence.</span></span>
* <span data-ttu-id="fd82e-124">適切な返す`IActionResult`です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-124">Return an appropriate `IActionResult`.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="fd82e-125">単体テスト</span><span class="sxs-lookup"><span data-stu-id="fd82e-125">Unit testing</span></span>

<span data-ttu-id="fd82e-126">[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)インフラストラクチャとの依存関係から分離でのアプリの一部のテストが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-126">[Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) involves testing a part of an app in isolation from its infrastructure and dependencies.</span></span> <span data-ttu-id="fd82e-127">単体テスト コント ローラー ロジック、1 つのアクションの内容のみをテストするときに、その依存関係や framework 自体の動作できません。</span><span class="sxs-lookup"><span data-stu-id="fd82e-127">When unit testing controller logic, only the contents of a single action is tested, not the behavior of its dependencies or of the framework itself.</span></span> <span data-ttu-id="fd82e-128">ユニットとしてテスト コント ローラーのアクションをその動作にだけ専念するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-128">As you unit test your controller actions, make sure you focus only on its behavior.</span></span> <span data-ttu-id="fd82e-129">単体テストをコント ローラーなどの回避[フィルター](filters.md)、[ルーティング](../../fundamentals/routing.md)、または[モデル バインディング](../models/model-binding.md)です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-129">A controller unit test avoids things like [filters](filters.md), [routing](../../fundamentals/routing.md), or [model binding](../models/model-binding.md).</span></span> <span data-ttu-id="fd82e-130">1 つだけをテストに焦点を当てた、単体テストは一般に書き込む単純なとを実行するクイックです。</span><span class="sxs-lookup"><span data-stu-id="fd82e-130">By focusing on testing just one thing, unit tests are generally simple to write and quick to run.</span></span> <span data-ttu-id="fd82e-131">多くのオーバーヘッドなし、適切に記述された一連の単体テストを頻繁に実行できます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-131">A well-written set of unit tests can be run frequently without much overhead.</span></span> <span data-ttu-id="fd82e-132">ただし、単体テストでは問題を検出されないコンポーネント間の対話での目的は[統合テスト](xref:mvc/controllers/testing#integration-testing)です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-132">However, unit tests do not detect issues in the interaction between components, which is the purpose of [integration testing](xref:mvc/controllers/testing#integration-testing).</span></span>

<span data-ttu-id="fd82e-133">単体テストする必要がありますカスタム フィルターやルートなどを記述している場合、特定のコント ローラー アクションで、テストの一部ではなく、します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-133">If you're writing custom filters, routes, etc, you should unit test them, but not as part of your tests on a particular controller action.</span></span> <span data-ttu-id="fd82e-134">これらは、分離環境でテストしてください。</span><span class="sxs-lookup"><span data-stu-id="fd82e-134">They should be tested in isolation.</span></span>

> [!TIP]
> <span data-ttu-id="fd82e-135">[作成し、Visual Studio での単体テストの実行](https://docs.microsoft.com/visualstudio/test/unit-test-your-code)です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-135">[Create and run unit tests with Visual Studio](https://docs.microsoft.com/visualstudio/test/unit-test-your-code).</span></span>

<span data-ttu-id="fd82e-136">単体テストを示すためには、次のコント ローラーを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-136">To demonstrate unit testing, review the following controller.</span></span> <span data-ttu-id="fd82e-137">ブレーンストーミング セッションの一覧を表示し、新しいブレーンストーミングを投稿して作成されるセッションを許可します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-137">It displays a list of brainstorming sessions and allows new brainstorming sessions to be created with a POST:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

<span data-ttu-id="fd82e-138">コント ローラーは、次の[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)のインスタンスに提供する依存関係の挿入を指定してください`IBrainstormSessionRepository`です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-138">The controller is following the [explicit dependencies principle](http://deviq.com/explicit-dependencies-principle/), expecting dependency injection to provide it with an instance of `IBrainstormSessionRepository`.</span></span> <span data-ttu-id="fd82e-139">これにより、非常に簡単にテストと同様に、モック オブジェクト フレームワークを使用して[Moq](https://www.nuget.org/packages/Moq/)です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-139">This makes it fairly easy to test using a mock object framework, like [Moq](https://www.nuget.org/packages/Moq/).</span></span> <span data-ttu-id="fd82e-140">`HTTP GET Index`メソッドにはないループおよび分岐のみ呼び出し 1 つのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="fd82e-140">The `HTTP GET Index` method has no looping or branching and only calls one method.</span></span> <span data-ttu-id="fd82e-141">これをテストする`Index`メソッド、ことを確認する必要があります、`ViewResult`返されると、`ViewModel`リポジトリから`List`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fd82e-141">To test this `Index` method, we need to verify that a `ViewResult` is returned, with a `ViewModel` from the repository's `List` method.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

<span data-ttu-id="fd82e-142">`HomeController` `HTTP POST Index` (上記) 方法を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd82e-142">The `HomeController` `HTTP POST Index` method (shown above) should verify:</span></span>

* <span data-ttu-id="fd82e-143">アクション メソッドは、無効な要求を返します`ViewResult`に適切なデータと`ModelState.IsValid`は`false`</span><span class="sxs-lookup"><span data-stu-id="fd82e-143">The action method returns a Bad Request `ViewResult` with the appropriate data when `ModelState.IsValid` is `false`</span></span>

* <span data-ttu-id="fd82e-144">`Add`リポジトリでメソッドが呼び出されると`RedirectToActionResult`が適切な引数と共に返されるときに`ModelState.IsValid`は true です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-144">The `Add` method on the repository is called and a `RedirectToActionResult` is returned with the correct arguments when `ModelState.IsValid` is true.</span></span>

<span data-ttu-id="fd82e-145">使用してエラーを追加することで無効なモデルの状態をテストできる`AddModelError`最初のテストを以下に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-145">Invalid model state can be tested by adding errors using `AddModelError` as shown in the first test below.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

<span data-ttu-id="fd82e-146">ときに最初のテストのことを確認`ModelState`が有効でない同じ`ViewResult`用として返される、`GET`要求します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-146">The first test confirms when `ModelState` is not valid, the same `ViewResult` is returned as for a `GET` request.</span></span> <span data-ttu-id="fd82e-147">テストが無効なモデルに渡すしようとしないに注意してください。</span><span class="sxs-lookup"><span data-stu-id="fd82e-147">Note that the test doesn't attempt to pass in an invalid model.</span></span> <span data-ttu-id="fd82e-148">はうまくままモデル バインディングが実行されていないので (ただし、[統合テスト](xref:mvc/controllers/testing#integration-testing)演習モデル バインディングを使用)。</span><span class="sxs-lookup"><span data-stu-id="fd82e-148">That wouldn't work anyway since model binding isn't running (though an [integration test](xref:mvc/controllers/testing#integration-testing) would use exercise model binding).</span></span> <span data-ttu-id="fd82e-149">この場合、モデル バインディングいないテスト中です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-149">In this case, model binding is not being tested.</span></span> <span data-ttu-id="fd82e-150">これらの単体テストでは、アクション メソッドのコードが何をテストするだけです。</span><span class="sxs-lookup"><span data-stu-id="fd82e-150">These unit tests are only testing what the code in the action method does.</span></span>

<span data-ttu-id="fd82e-151">2 番目のテストの検証時に`ModelState`が有効で、新しい`BrainstormSession`(リポジトリ) を使用して追加が返されます、`RedirectToActionResult`予期されたプロパティを持つ。</span><span class="sxs-lookup"><span data-stu-id="fd82e-151">The second test verifies that when `ModelState` is valid, a new `BrainstormSession` is added (via the repository), and the method returns a `RedirectToActionResult` with the expected properties.</span></span> <span data-ttu-id="fd82e-152">呼び出されないモックの呼び出しは、通常は無視されますが、呼び出し`Verifiable`セットアップの最後の呼び出しで許可されているテストで検証します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-152">Mocked calls that aren't called are normally ignored, but calling `Verifiable` at the end of the setup call allows it to be verified in the test.</span></span> <span data-ttu-id="fd82e-153">これは、呼び出したときに`mockRepo.Verify`、予想されるメソッドが呼び出されなかった場合、テストが失敗します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-153">This is done with the call to `mockRepo.Verify`, which will fail the test if the expected method was not called.</span></span>

> [!NOTE]
> <span data-ttu-id="fd82e-154">このサンプルで使用される Moq ライブラリしやすい検証不可能のモック (「厳密でない」モックまたはスタブとも呼ばれます) を使用して、検証可能なまたは"strict"、モックを混在させます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-154">The Moq library used in this sample makes it easy to mix verifiable, or "strict", mocks with non-verifiable mocks (also called "loose" mocks or stubs).</span></span> <span data-ttu-id="fd82e-155">詳細については[Moq とモック動作をカスタマイズする](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-155">Learn more about [customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).</span></span>

<span data-ttu-id="fd82e-156">アプリで別のコント ローラーには、特定のブレーンストーミング セッションに関連する情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-156">Another controller in the app displays information related to a particular brainstorming session.</span></span> <span data-ttu-id="fd82e-157">無効な id 値を扱うためのロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fd82e-157">It includes some logic to deal with invalid id values:</span></span>

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

<span data-ttu-id="fd82e-158">コント ローラーのアクションが 3 つのケースをテストする場合に、1 つずつ`return`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="fd82e-158">The controller action has three cases to test, one for each `return` statement:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

<span data-ttu-id="fd82e-159">アプリでは、web API (ブレーンストーミング セッションと新しいアイデアをセッションに追加するためのメソッドに関連付けられているアイデアの一覧) と機能を公開します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-159">The app exposes functionality as a web API (a list of ideas associated with a brainstorming session and a method for adding new ideas to a session):</span></span>

<a name="ideas-controller"></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

<span data-ttu-id="fd82e-160">`ForSession`メソッドの一覧を返します`IdeaDTO`型です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-160">The `ForSession` method returns a list of `IdeaDTO` types.</span></span> <span data-ttu-id="fd82e-161">API 呼び出し経由で直接、ビジネス ドメイン エンティティが返されないように、頻繁に以降 API クライアントを必要として、アプリの内部ドメイン モデル外部に公開する API で不必要に結合するより多くのデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-161">Avoid returning your business domain entities directly via API calls, since frequently they include more data than the API client requires, and they unnecessarily couple your app's internal domain model with the API you expose externally.</span></span> <span data-ttu-id="fd82e-162">ドメイン エンティティと、ネットワーク経由で戻ります型間のマッピングを手動で行うことができます (LINQ を使用して`Select`次のように) のようなライブラリを使用してまたは[AutoMapper](https://github.com/AutoMapper/AutoMapper)</span><span class="sxs-lookup"><span data-stu-id="fd82e-162">Mapping between domain entities and the types you will return over the wire can be done manually (using a LINQ `Select` as shown here) or using a library like [AutoMapper](https://github.com/AutoMapper/AutoMapper)</span></span>

<span data-ttu-id="fd82e-163">用の単体テスト、`Create`と`ForSession`API メソッド。</span><span class="sxs-lookup"><span data-stu-id="fd82e-163">The unit tests for the `Create` and `ForSession` API methods:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

<span data-ttu-id="fd82e-164">既に説明したよう、メソッドの動作をテストするときに`ModelState`が有効でない、テストの一部としてコント ローラーにモデル エラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-164">As stated previously, to test the behavior of the method when `ModelState` is invalid, add a model error to the controller as part of the test.</span></span> <span data-ttu-id="fd82e-165">モデルの検証またはモデルのバインディングを単体テストでテスト - 特定が増したときのアクション メソッドの動作をテストするということは避けます`ModelState`値。</span><span class="sxs-lookup"><span data-stu-id="fd82e-165">Don't try to test model validation or model binding in your unit tests - just test your action method's behavior when confronted with a particular `ModelState` value.</span></span>

<span data-ttu-id="fd82e-166">2 番目のテストは、null を返します、モック リポジトリが構成されているため、null を返して、リポジトリに依存します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-166">The second test depends on the repository returning null, so the mock repository is configured to return null.</span></span> <span data-ttu-id="fd82e-167">テスト データベースを作成する必要はありません (メモリ内またはそれ以外の場合) をこの結果を返すクエリを構築および - ようにには、単一のステートメントで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-167">There's no need to create a test database (in memory or otherwise) and construct a query that will return this result - it can be done in a single statement as shown.</span></span>

<span data-ttu-id="fd82e-168">前回のテストであることを確認、リポジトリの`Update`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-168">The last test verifies that the repository's `Update` method is called.</span></span> <span data-ttu-id="fd82e-169">モックがで呼び出された以前と同様、`Verifiable`とし、モック リポジトリの`Verify`メソッドが呼び出され、検証可能なメソッドが実行されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-169">As we did previously, the mock is called with `Verifiable` and then the mocked repository's `Verify` method is called to confirm the verifiable method was executed.</span></span> <span data-ttu-id="fd82e-170">いることを確認する単体テストの責任ではありません、`Update`メソッドは、データを保存; 統合テストを実行できます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-170">It's not a unit test responsibility to ensure that the `Update` method saved the data; that can be done with an integration test.</span></span>

## <a name="integration-testing"></a><span data-ttu-id="fd82e-171">統合テスト</span><span class="sxs-lookup"><span data-stu-id="fd82e-171">Integration testing</span></span>

<span data-ttu-id="fd82e-172">[統合テスト](../../testing/integration-testing.md)は、アプリ内の個別のモジュールを正常に確実に行われます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-172">[Integration testing](../../testing/integration-testing.md) is done to ensure separate modules within your app work correctly together.</span></span> <span data-ttu-id="fd82e-173">一般に、単体テストをテストすることが何も、テストすることも、統合テストでが逆で true はありません。</span><span class="sxs-lookup"><span data-stu-id="fd82e-173">Generally, anything you can test with a unit test, you can also test with an integration test, but the reverse isn't true.</span></span> <span data-ttu-id="fd82e-174">ただし、統合テストは単体テストよりも非常に低くなる傾向があります。</span><span class="sxs-lookup"><span data-stu-id="fd82e-174">However, integration tests tend to be much slower than unit tests.</span></span> <span data-ttu-id="fd82e-175">したがって、何でも単体テストの場合は、して統合テストを使用して、複数の共同作業者に関連するシナリオをテストすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-175">Thus, it's best to test whatever you can with unit tests, and use integration tests for scenarios that involve multiple collaborators.</span></span>

<span data-ttu-id="fd82e-176">便利可能性があります、モック オブジェクトがほとんど統合テストで使用します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-176">Although they may still be useful, mock objects are rarely used in integration tests.</span></span> <span data-ttu-id="fd82e-177">単体テストでのモック オブジェクトは、テストの目的でテストされている単体の外部での共同作業者の動作を制御する効果的な方法です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-177">In unit testing, mock objects are an effective way to control how collaborators outside of the unit being tested should behave for the purposes of the test.</span></span> <span data-ttu-id="fd82e-178">統合テストで実際のコラボレーターを使用して、全体のサブシステムが正しくが連携することを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-178">In an integration test, real collaborators are used to confirm the whole subsystem works together correctly.</span></span>

### <a name="application-state"></a><span data-ttu-id="fd82e-179">アプリケーションの状態</span><span class="sxs-lookup"><span data-stu-id="fd82e-179">Application state</span></span>

<span data-ttu-id="fd82e-180">1 つの重要な考慮事項統合テストを実行するときは、アプリの状態を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-180">One important consideration when performing integration testing is how to set your app's state.</span></span> <span data-ttu-id="fd82e-181">テストが、互いに独立して実行する必要があり、各テストが既知の状態でアプリを開始する必要がありますのでします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-181">Tests need to run independent of one another, and so each test should start with the app in a known state.</span></span> <span data-ttu-id="fd82e-182">アプリがデータベースを使用するすべての永続化、または問題にこの可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd82e-182">If your app doesn't use a database or have any persistence, this may not be an issue.</span></span> <span data-ttu-id="fd82e-183">ただし、ほとんどのアプリで現実世界では、1 つのテストによって行われた変更は、データ ストアをリセットしない限り、別のテストに影響がようににいくつかの種類のデータ ストアにその状態が保持されます。</span><span class="sxs-lookup"><span data-stu-id="fd82e-183">However, most real-world apps persist their state to some kind of data store, so any modifications made by one test could impact another test unless the data store is reset.</span></span> <span data-ttu-id="fd82e-184">組み込みを使用して`TestServer`は、統合テスト内で ASP.NET Core アプリケーションをホストする非常に簡単ですが、使用するデータへのアクセスを許可しないとは限りません。</span><span class="sxs-lookup"><span data-stu-id="fd82e-184">Using the built-in `TestServer`, it's very straightforward to host ASP.NET Core apps within our integration tests, but that doesn't necessarily grant access to the data it will use.</span></span> <span data-ttu-id="fd82e-185">1 つの方法は、テスト データベースに接続するアプリがあるを実際のデータベースを使用している場合は、各テストの実行前に、既知の状態にリセットしてテストできますにアクセスすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-185">If you're using an actual database, one approach is to have the app connect to a test database, which your tests can access and ensure is reset to a known state before each test executes.</span></span>

<span data-ttu-id="fd82e-186">このサンプル アプリケーションで使用している Entity Framework Core InMemoryDatabase サポート、だけテスト プロジェクトからそれに接続できないようにします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-186">In this sample application, I'm using Entity Framework Core's InMemoryDatabase support, so I can't just connect to it from my test project.</span></span> <span data-ttu-id="fd82e-187">公開は、代わりに、`InitializeDatabase`メソッドから、アプリの`Startup`クラスは、アプリの起動時にある場合にお電話させて、`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="fd82e-187">Instead, I expose an `InitializeDatabase` method from the app's `Startup` class, which I call when the app starts up if it's in the `Development` environment.</span></span> <span data-ttu-id="fd82e-188">統合テストに自動的にパフォーマンスが向上環境を設定する限り`Development`です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-188">My integration tests automatically benefit from this as long as they set the environment to `Development`.</span></span> <span data-ttu-id="fd82e-189">InMemoryDatabase はアプリを再起動するたびにリセットされた後、データベースのリセットについて心配する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fd82e-189">I don't have to worry about resetting the database, since the InMemoryDatabase is reset each time the app restarts.</span></span>

<span data-ttu-id="fd82e-190">`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="fd82e-190">The `Startup` class:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

<span data-ttu-id="fd82e-191">表示されます、`GetTestSession`以下の統合テストで頻繁に使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="fd82e-191">You'll see the `GetTestSession` method used frequently in the integration tests below.</span></span>

### <a name="accessing-views"></a><span data-ttu-id="fd82e-192">ビューへのアクセス</span><span class="sxs-lookup"><span data-stu-id="fd82e-192">Accessing views</span></span>

<span data-ttu-id="fd82e-193">各統合テスト クラスの構成、 `TestServer` ASP.NET Core アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-193">Each integration test class configures the `TestServer` that will run the ASP.NET Core app.</span></span> <span data-ttu-id="fd82e-194">既定では、`TestServer`が実行されている - この場合、テスト プロジェクト フォルダーのフォルダーに web アプリケーションをホストします。</span><span class="sxs-lookup"><span data-stu-id="fd82e-194">By default, `TestServer` hosts the web app in the folder where it's running - in this case, the test project folder.</span></span> <span data-ttu-id="fd82e-195">したがって、しようとするテスト コント ローラーのアクションを返す`ViewResult`、このエラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd82e-195">Thus, when you attempt to test controller actions that return `ViewResult`, you may see this error:</span></span>

```
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

<span data-ttu-id="fd82e-196">この問題を解決するには、テスト対象プロジェクトのビューを見つけることができるように、サーバーのコンテンツのルートを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd82e-196">To correct this issue, you need to configure the server's content root, so it can locate the views for the project being tested.</span></span> <span data-ttu-id="fd82e-197">これへの呼び出しで`UseContentRoot`で、`TestFixture`次に示すクラス。</span><span class="sxs-lookup"><span data-stu-id="fd82e-197">This is done by a call to `UseContentRoot` in the `TestFixture` class, shown below:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

<span data-ttu-id="fd82e-198">`TestFixture`クラスは、構成および作成を担当する、`TestServer`を設定、`HttpClient`通信するために、`TestServer`です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-198">The `TestFixture` class is responsible for configuring and creating the `TestServer`, setting up an `HttpClient` to communicate with the `TestServer`.</span></span> <span data-ttu-id="fd82e-199">使用して、統合の各テスト、`Client`プロパティをテスト サーバーに接続し、要求を行います。</span><span class="sxs-lookup"><span data-stu-id="fd82e-199">Each of the integration tests uses the `Client` property to connect to the test server and make a request.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

<span data-ttu-id="fd82e-200">上記の最初のテスト、`responseString`ビューで、期待どおりの結果が含まれていることを確認するを検査することができますの実際のレンダリングされる HTML を保持します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-200">In the first test above, the `responseString` holds the actual rendered HTML from the View, which can be inspected to confirm it contains expected results.</span></span>

<span data-ttu-id="fd82e-201">2 番目のテストはフォーム ポストを一意のセッションの名前を構築、アプリにポストし、必要なリダイレクトが返されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-201">The second test constructs a form POST with a unique session name and POSTs it to the app, then verifies that the expected redirect is returned.</span></span>

### <a name="api-methods"></a><span data-ttu-id="fd82e-202">API のメソッド</span><span class="sxs-lookup"><span data-stu-id="fd82e-202">API methods</span></span>

<span data-ttu-id="fd82e-203">アプリが web Api では、そのテストを自動化されていることをお勧めは期待どおりに実行することを確認を公開するかどうか。</span><span class="sxs-lookup"><span data-stu-id="fd82e-203">If your app exposes web APIs, it's a good idea to have automated tests confirm they execute as expected.</span></span> <span data-ttu-id="fd82e-204">組み込み`TestServer`web Api をテストするが容易です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-204">The built-in `TestServer` makes it easy to test web APIs.</span></span> <span data-ttu-id="fd82e-205">常に確認する必要がある場合は、API のメソッドは、モデル バインディングを使用している、 `ModelState.IsValid`、統合テストは、適切な場所に、モデルの検証が正しく動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-205">If your API methods are using model binding, you should always check `ModelState.IsValid`, and integration tests are the right place to confirm that your model validation is working properly.</span></span>

<span data-ttu-id="fd82e-206">次のテストの対象のセット、`Create`メソッドで、 [IdeasController](xref:mvc/controllers/testing#ideas-controller)上に示したクラス。</span><span class="sxs-lookup"><span data-stu-id="fd82e-206">The following set of tests target the `Create` method in the [IdeasController](xref:mvc/controllers/testing#ideas-controller) class shown above:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

<span data-ttu-id="fd82e-207">HTML ビューを返すアクションの統合テストとは異なり web API メソッド結果を返す通常できます厳密に型指定のオブジェクトに逆シリアル化されたように上記の最後のテストを示しています。</span><span class="sxs-lookup"><span data-stu-id="fd82e-207">Unlike integration tests of actions that returns HTML views, web API methods that return results can usually be deserialized as strongly typed objects, as the last test above shows.</span></span> <span data-ttu-id="fd82e-208">ここでは、テストが結果を逆シリアル化、`BrainstormSession`インスタンス、およびアイデアがアイデアのコレクションに正しく追加されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd82e-208">In this case, the test deserializes the result to a `BrainstormSession` instance, and confirms that the idea was correctly added to its collection of ideas.</span></span>

<span data-ttu-id="fd82e-209">この記事の内容の統合テストの他の例があります[サンプル プロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)です。</span><span class="sxs-lookup"><span data-stu-id="fd82e-209">You'll find additional examples of integration tests in this article's [sample project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).</span></span>