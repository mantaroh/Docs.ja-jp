---
title: "ASP.NET Core でスキャフォールディングされた Razor ページ"
author: rick-anderson
description: "スキャフォールディングによって生成された Razor ページについて説明します。"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 1b7a41a0e1383ba287e4ddce53b33761a96a3129
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET Core でスキャフォールディングされた Razor ページ

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、[前のチュートリアル](xref:tutorials/razor-pages/page)でスキャフォールディングによって作成された Razor ページについて説明します。 

サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します。

## <a name="the-create-delete-details-and-edit-pages"></a>[作成]、[削除]、[詳細]、および [編集] ページ

*Pages/Movies/Index.cshtml.cs* という分離コード ファイルを確認します。[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml.cs)]

Razor ページは `PageModel` から派生します。 慣例により、`PageModel` 派生クラスは `<PageName>Model` と呼ばれます。 コンストラクターは[依存性の注入](xref:fundamentals/dependency-injection)を使用して、`MovieContext` をページに追加します。 スキャフォールディングされたページではすべてこのパターンに従います。

ページに対して要求が行われると、`OnGetAsync` メソッドは Razor ページにムービーのリストを返します。 Razor ページで `OnGetAsync` または `OnGet` が呼び出され、ページの状態が初期化されます。 この場合、`OnGetAsync` で表示するムービーのリストを取得します。

次のように、*Pages/Movies/Index.cshtml* Razor ページを確認します。

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml)]

Razor では、HTML から C# または Razor 固有のマークアップに移行できます。 `@` シンボルの後に [Razor で予約済みのキーワード](xref:mvc/views/razor#razor-reserved-keywords)が続いている場合は、Razor 固有のマークアップに移行します。それ以外の場合は、C# に移行します。

`@page` Razor ディレクティブはファイルを MVC アクションに分割します。これは、要求を処理できることを意味します。 `@page` はページで最初の Razor ディレクティブである必要があります。 `@page` は、Razor 固有のマークアップへの移行の例です。 詳細については、「[Razor syntax](xref:mvc/views/razor#razor-syntax)」 (Razor の構文) を参照してください。

次の HTML ヘルパーで使用されるラムダ式を確認します。

`@Html.DisplayNameFor(model => model.Movie[0].Title))`

`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。 ラムダ式は評価されるのではなく検査されます。 これは、`model`、`model.Movies`、または `model.Movies[0]` が `null` または空である場合、アクセス違反がないことを意味します。 ラムダ式が (`@Html.DisplayFor(modelItem => item.Title)` などを使用して) 評価される場合は、モデルのプロパティ値が評価されます。

<a name="md"></a>
### <a name="the-model-directive"></a>@model ディレクティブ

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-2&highlight=2)]

`@model` ディレクティブは、Razor ページに渡されるモデルの型を指定します。 前の例の `@model` 行は、Razor ページで `PageModel` 派生クラスを使用できるようにします。 モデルは、ページの `@Html.DisplayNameFor` および `@Html.DisplayName` [HTML ヘルパーで](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)使用されます。

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData とレイアウト

次のコードがあるとします。

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-6&highlight=4-)]

上の強調表示されたコードは、Razor の C# への移行例です。 `{` 文字と `}` 文字で C# コードのブロックを囲みます。

`Controller` 基本クラスには `ViewData` 辞書プロパティがあり、これを使用して、ビューに渡すデータを追加できます。 キー/値のパターンを使用して、`ViewData` 辞書にオブジェクトを追加します。 上のサンプルでは、"Title" プロパティが `ViewData` 辞書に追加されます。 "Title" プロパティは *Pages/_Layout.cshtml* ファイルで使用されます。 次のマークアップは、*Pages/_Layout.cshtml* ファイルの最初の数行を示しています。

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

`@*Markup removed for brevity.*@` 行は Razor コメントです。 HTML コメント (`<!-- -->`) とは異なり、Razor コメントはクライアントには送信されません。

アプリを実行して、プロジェクトのリンク (**[ホーム]**、**[バージョン情報]**、**[連絡先]**、**[作成]**、**[編集]**、および **[削除]**) をテストします。 各ページで、ブラウザー タブで表示できるタイトルを設定します。ページをブックマークすると、ブックマークでタイトルが使用されます。 現在、*Pages/Index.cshtml* と *Pages/Movies/Index.cshtml* のタイトルは同じですが、変更して別の値を指定することができます。

`Layout` プロパティは *Pages/_ViewStart.cshtml* ファイルで設定されています。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

上のマークアップでは、*Pages* フォルダーの下のすべての Razor ファイルの *Pages/_Layout.cshtml* にレイアウト ファイルを設定します。 詳細については、「[Layout](xref:mvc/razor-pages/index#layout)」 (レイアウト) を参照してください。

### <a name="update-the-layout"></a>レイアウトの更新

より短い文字列を使用するために、*Pages/_Layout.cshtml* ファイルの `<title>` 要素を変更します。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6-)]

*Pages/_Layout.cshtml* ファイルで次のアンカー要素を見つけます。

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
上の要素を次のマークアップに置き換えます。

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

上のアンカー要素は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。 この場合は、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)です。 `asp-page="/Movies/Index"` タグ ヘルパー属性と値で、`/Movies/Index` Razor ページへのリンクが作成されます。

変更内容を保存し、**RpMovie** リンクをクリックしてアプリをテストします。 GitHub の [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) ファイルを参照してください。

### <a name="the-create-code-behind-page"></a>[作成] 分離コード ページ

次のように、*Pages/Movies/Create.cshtml.cs* という分離コード ファイルを確認します。

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetALL)]

`OnGet` メソッドは、ページに必要な状態を初期化します。 [作成] ページには初期化する状態はありません。 `Page` メソッドでは、*Create.cshtml* ページをレンダリングする `PageResult` オブジェクトが作成されます。

`Movie` プロパティは `[BindProperty]` 属性を使用して、[モデル バインド](xref:mvc/models/model-binding)にオプトインします。 [作成] フォームでフォーム値が投稿されると、ASP.NET Core ランタイムが投稿された値を `Movie` モデルにバインドします。

`OnPostAsync` メソッドは、ページでフォーム データが投稿されたときに実行されます。

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetPost)]

モデル エラーがある場合は、投稿されたフォーム データと共にフォームが再表示されます。 ほとんどのモデル エラーは、フォームが投稿される前にクライアント側でキャッチできます。 モデル エラーの例では、日付に変換できない日付フィールドの値が投稿されています。 クライアント側の検証とモデルの検証については、チュートリアルの後半で詳しく説明します。

モデル エラーがない場合、データは保存され、ブラウザーはインデックス ページにリダイレクトされます。

### <a name="the-create-razor-page"></a>[作成] Razor ページ

次のように、*Pages/Movies/Create.cshtml* Razor ページ ファイルを確認します。

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml)]

Visual Studio に、タグ ヘルパーで使用される独特なフォントで `<form method="post">` タグが表示されます。 `<form method="post">` 要素は[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)です。 フォーム タグ ヘルパーには自動的に[偽造防止トークン](xref:security/anti-request-forgery)が含まれます。

![Create.cshtml ページの VS17 ビュー](page/_static/th.png)

スキャフォールディング エンジンは、次のような、(ID を除く) モデルの各フィールドの Razor マークアップを作成します。

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml?range=15-20)]

[検証タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` と ` <span asp-validation-for`) には検証エラーが表示されます。 検証については、後で詳しく説明します。

[ラベル タグ ヘルパー](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) は、`Title` プロパティのラベル キャプションと `for` 属性を生成します。

[入力タグ ヘルパー](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) は [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 属性を使用し、クライアント側で jQuery 検証に必要な HTML 属性を生成します。

次のチュートリアルでは、SQL Server LocalDB とデータベースのシード処理について説明します。

>[!div class="step-by-step"]
[前: モデルの追加](xref:tutorials/razor-pages/modelz)
[次: SQL Server LocalDB](xref:tutorials/razor-pages/sql)