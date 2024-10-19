
# Bootstrap UI

[![Build Status][ico-ga]][ga]
[![Coverage Status][ico-coverage]][coverage]
[![Total Downloads][ico-downloads]][package]
[![License][ico-license]][license]

[ico-ga]: https://img.shields.io/github/actions/workflow/status/FriendsOfCake/bootstrap-ui/ci.yml?branch=master&style=flat-square
[ico-coverage]: https://img.shields.io/codecov/c/github/FriendsOfCake/bootstrap-ui.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/friendsofcake/bootstrap-ui.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square

[ga]: https://github.com/FriendsOfCake/bootstrap-ui/actions?query=workflow%3ACI+branch%3Amaster
[coverage]: https://codecov.io/github/FriendsOfCake/bootstrap-ui
[package]: https://packagist.org/packages/friendsofcake/bootstrap-ui
[license]: LICENSE.txt

[Bootstrap 5][bs] を [CakePHP 5][cakephp] でシームレスに使用します。

バージョン情報については、[version map](https://github.com/FriendsOfCake/bootstrap-ui/wiki#version-map) を参照してください。

＃＃ 必要条件 (Requirements)

* CakePHP 5.x
* Bootstrap 5.x
* npm 6.x
* Popper.js 2.x
* Bootstrap Icons 1.5.x

## 何が含まれていますか? (What's included?)

- FlashHelper (element types: `error`, `info`, `success`, `warning`)
- FormHelper (align: `default`, `inline`, `horizontal`)
- BreadcrumbsHelper
- HtmlHelper (components: `badge`, `icon`)
- PaginatorHelper
- Widgets (`basic`, `button`, `datetime`, `file`, `select`, `textarea`)
- Example layouts (`cover`, `signin`, `dashboard`)
- Bake templates

＃＃ 目次

- [インストール](#installation)
- [セットアップ](#setup)
  - [Bootstrap コマンドの使用](#using-the-bootstrap-commands)
  - [手動セットアップ](#manual-setup)
  - [BootstrapUI レイアウト](#bootstrapui-layouts)
  - [Bootstrap フレームワークの組み込み](#including-the-bootstrap-framework)
- [テンプレートをベイクする](#bake-templates)
- [使用方法](#usage)
- [貢献](#貢献)
- [ライセンス](#license)

## インストール(Installation)

`cd` でアプリフォルダのルート (`composer.json` ファイルがある場所) に移動し、次の [Composer][composer] を実行します。
command:

```
composer require friendsofcake/bootstrap-ui
```

Then load the plugin using CakePHP's console:

```
bin/cake plugin load BootstrapUI
```

＃＃ 設定(Setup)

Bootstrapのコマンドを使用して必要な変更を行うか、手動で行うこともできます。

### Bootstrapコマンドの使用

1.Bootstrapのアセット（BootstrapのCSS/JSファイル、Popper.js）をnpm経由でインストールするには、installコマンドを使用するか、手動でインストールすることもできます。[install them manually](#installing-bootstrap-assets-via-npm):

   ```
   bin/cake bootstrap install
   ```

   これにより、すべてのアセットが取得され、配布アセットがBootstrapUIプラグインのwebrootディレクトリにコピーされ、アプリケーションの `webroot` ディレクトリにシンボリックリンク（またはコピー）されます。

   アセットの正確な固定バージョンではなく、最新のマイナーバージョンをインストールしたい場合は、`--latest` オプションを使用できます。

   ```
   bin/cake bootstrap install --latest
   ```

2. `src/View/AppView` クラスを変更して `BootstrapUI\View\UIView` を拡張するか、
   特性 `BootStrapUI\View\UIViewTrait` を使用します。これを行うには、`modify_view` コマンドを使用するか、手動でビューを変更してください
   [change your view manually](#appview-setup-using-uiview):

   ```
   bin/cake bootstrap modify_view
   ```

   これにより、[AppView setup using UIView](#appview-setup-using-uiview) で説明されているように、`src/View/AppView` が書き換えられます。

3. BootstrapUIにはいくつかのサンプルレイアウトが付属しています。`copy_layouts`コマンドを使用してインストールするか、手動でコピーすることができます。
   [copy them manually](#copying-example-layouts):

   ```
   bin/cake bootstrap copy_layouts
   ```

   これにより、3つのサンプルレイアウト`cover.php`、`dashboard.php`、`signin.php`がアプリケーションの
   `src/templates/layout/TwitterBootstrap` です。
   これにより、3つのレイアウト例である `cover.php`、`dashboard.php`、`signin.php` が、アプリケーションの `src/templates/layout/TwitterBootstrap` ディレクトリにコピーされます。

### 手動設定

#### npm 経由で Bootstrap アセットをインストールする

[`install` コマンド](#using-the-bootstrap-commands) は [npm] 経由で Bootstrap アセットをインストールします。
どのアセットを含めるか、どこに配置するかを制御したい場合は手動で実行してください。

[the `install` command](#using-the-bootstrap-commands) install コマンド は、Bootstrapのアセットを[npm]を通じてインストールします。アセットの内容や配置場所を制御したい場合は、手動でインストールすることも可能です。

以下のコマンドは、アプリケーションの root にいると仮定します:

```
npm install @popperjs/core@2 bootstrap@5 bootstrap-icons@1
mkdir -p webroot/css
mkdir -p webroot/font/fonts
mkdir -p webroot/js
cp node_modules/@popperjs/core/dist/umd/popper.js webroot/js
cp node_modules/@popperjs/core/dist/umd/popper.min.js webroot/js
cp node_modules/bootstrap/dist/css/bootstrap.css webroot/css/
cp node_modules/bootstrap/dist/css/bootstrap.min.css webroot/css/
cp node_modules/bootstrap/dist/js/bootstrap.js webroot/js/
cp node_modules/bootstrap/dist/js/bootstrap.min.js webroot/js/
cp node_modules/bootstrap-icons/font/bootstrap-icons.css webroot/font/
cp node_modules/bootstrap-icons/font/fonts/bootstrap-icons.woff webroot/font/fonts/
cp node_modules/bootstrap-icons/font/fonts/bootstrap-icons.woff2 webroot/font/fonts/
cp vendor/friendsofcake/bootstrap-ui/webroot/font/bootstrap-icon-sizes.css webroot/font/
```

#### UIView を使用した AppView のセットアップ

簡単にセットアップするには、`AppView`クラスを `BootstrapUI\View\UIView` に拡張するだけです。この基底クラスが、初期化処理とアプリ用のBootstrapUIの`default.php`レイアウトの読み込みを行います。

`src\View\AppView.php` は次のようになります。

```php
declare(strict_types=1);

namespace App\View;

use BootstrapUI\View\UIView;

class AppView extends UIView
{
    /**
     * Initialization hook method.
     */
    public function initialize(): void
    {
        // Don't forget to call parent::initialize()
        parent::initialize();
    }
}
```

#### UIViewTrait を使用した AppView のセットアップ

既存のアプリケーションにBootstrapUIを追加する場合、トレイトを使用する方がレイアウトの読み込みをより細かく制御できるため、便利です。以下はその例です：

```php
declare(strict_types=1);

namespace App\View;

use BootstrapUI\View\UIViewTrait;
use Cake\View\View;

class AppView extends View
{
    use UIViewTrait;

    /**
     * Initialization hook method.
     */
    public function initialize(): void
    {
        parent::initialize();

        // Call the initializeUI method from UIViewTrait
        $this->initializeUI();
    }
}
```

#### サンプルレイアウトのコピー

BootstrapUIのサンプルレイアウト（Bootstrapのサンプルから直接取得したもの）を使用するには、
アプリケーションのレイアウトディレクトリにコピーするには、
[the `copy_layouts` command](#using-the-bootstrap-commands)を使用するか、手動でファイルをコピーします。

```
cp -R vendor/friendsofcake/bootstrap-ui/templates/layout/examples templates/layout/TwitterBootstrap
```

### BootstrapUIレイアウト

BootstrapUI には、独自の `default.php` レイアウト ファイルと、Bootstrap フレームワークから取得した例が付属しています。

ビューのレイアウトが定義されていない場合、`BootstrapUI\View\UIViewTrait` は独自の `default.php` レイアウト ファイルをロードします。
この動作をオーバーライドするには 2 つの方法があります。

- `$this->setLayout('layout')` を使用してテンプレートにレイアウトを割り当てます。
- `AppView` の `initialize()` 関数に `$this->initializeUI(['layout' => false]);` を追加して、`BootstrapUI\View\UIViewTrait` でのレイアウトの自動読み込みを無効にします。

#### サンプルレイアウトの使用

アプリケーションのレイアウトディレクトリにコピーしたら（
[the `copy_layouts` command](#using-the-bootstrap-commands) または [manually](#copying-example-layouts))、単に次のようにビュー内のサンプルレイアウトを拡張します。

```
$this->extend('../layout/TwitterBootstrap/dashboard');
```

利用可能なサンプルは次のとおりです:

- `cover`
- `signin`
- `dashboard`

**注意: コピーしたレイアウトでスタイルシートを設定することを忘れないでください。**

### Bootstrapフレームワークの組み込み

[the BoostrapUI plugin's default layout](#bootstrapui-layouts)を使用しており、Bootstrap
[the `install` command](#using-the-bootstrap-commands)を使用してアセットをインストールすると、必要なアセットは自動的に含まれています。

独自のレイアウト テンプレートを使用する場合は、必要な CSS/JS ファイルを自分で含める必要があります。

[the `install` command](#using-the-bootstrap-commands)を使用してアセットをインストールした場合は、
標準のプラグイン構文を使用してこれらを実行します。

```php
// in the <head>
echo $this->Html->css('BootstrapUI.bootstrap.min');
echo $this->Html->css(['BootstrapUI./font/bootstrap-icons', 'BootstrapUI./font/bootstrap-icon-sizes']);
echo $this->Html->script(['BootstrapUI.popper.min', 'BootstrapUI.bootstrap.min']);
```

アセットを手動でインストールした場合は、それに応じてパスを使用する必要があります。
[the example copy commands](#installing-bootstrap-assets-via-npm) では、標準の短いパス構文を使用できます。

```php
echo $this->Html->css('bootstrap.min');
echo $this->Html->css(['/font/bootstrap-icons', '/font/bootstrap-icon-sizes']);
echo $this->Html->script(['popper.min', 'bootstrap.min']);
```

CakePHP の規則に従わないパスを使用する場合は、明示的に指定する必要があります。

```php
echo $this->Html->css('/path/to/bootstrap.css');
echo $this->Html->css(['/path/to/bootstrap-icons.css', '/path/to/bootstrap-icon-sizes.css']);
echo $this->Html->script(['/path/to/popper.js', '/path/to/bootstrap.js']);
```

## テンプレートをベイクする(Bake templates)

さらに自動化したい人のために、ベイク テンプレートがいくつか用意されています。次のように使用します。

```
bin/cake bake [subcommand] -t BootstrapUI
```

現在、次の bake subcommands が bake テンプレートに含まれています。

### `template`

デフォルトの`index`、`add`、`edit`、`view`テンプレートに加えて、`login`テンプレートも利用できます。
デフォルトの CRUD アクション ビュー テンプレートは次のように利用できます。

```bash
bin/cake bake template ControllerName -t BootstrapUI
```

`login` テンプレートは、アクション名を指定して明示的に使用する必要があります。

```bash
bin/cake bake template ControllerName login -t BootstrapUI
```

＃＃ 使用法(Usage)

BootstrapUIの核心は、CakePHPのコアヘルパーに対する拡張機能の集まりです。これらのヘルパーは、ビュー要素のレンダリングに使用されるHTMLテンプレートを置き換えます。これにより、Bootstrapのスタイルを使用したフォームやコンポーネントを作成することができます。

現在拡張されているヘルパーの一覧は以下の通りです：

- `BootstrapUI\View\Helper\FlashHelper`
- `BootstrapUI\View\Helper\FormHelper`
- `BootstrapUI\View\Helper\HtmlHelper`
- `BootstrapUI\View\Helper\PaginatorHelper`
- `BootstrapUI\View\Helper\BreadcrumbsHelper`

`BootstrapUI\View\UIViewTrait`が初期化されると、上記のヘルパーがCakePHPコアヘルパーと同じエイリアスで読み込まれます。つまり、ビューで`$this->Form->create()`を使用する際、使用されるヘルパーはBootstrapUIプラグインのものになります。

### 基本フォーム(Basic forms)

```php
echo $this->Form->create($article);
echo $this->Form->control('title');
echo $this->Form->control('published', ['type' => 'checkbox']);
echo $this->Form->button('Submit');
echo $this->Form->end();
```

以下のHTMLがレンダリングされます：

```html
<form method="post" accept-charset="utf-8" action="/articles/add">
    <!-- ... -->
    <div class="mb-3 form-group text">
        <label class="form-label" for="title">Title</label>
        <input type="text" name="title" id="title" class="form-control">
    </div>
    <div class="mb-3 form-group form-check checkbox">
        <input type="hidden" name="published" value="0">
        <input type="checkbox" class="form-check-input" name="published" value="1" id="published">
        <label class="form-check-label" for="published">Published</label>
    </div>
    <button type="submit" class="btn btn-secondary">Submit</button>
    <!-- ... -->
</form>
```

### 水平フォーム(Horizontal forms)

水平フォームでは、ラベルとコントロールが自動的に別々の列にレンダリングされます（該当する場合）。ラベルは最初の列に、コントロールは2番目の列に配置されます。

配置は`align`オプションで設定できます。`md`の列サイズのリストか
[Bootstrap 画面サイズ/ブレークポイント](https://getbootstrap.com/docs/5.0/layout/breakpoints/)、またはマトリックス
画面サイズ/ブレークポイント名と列サイズ。

配置は、`align`オプションで設定できます。このオプションには、`md`
[Bootstrap screen-size/breakpoint](https://getbootstrap.com/docs/5.0/layout/breakpoints/)向けの列サイズのリスト、または画面サイズ/ブレークポイント名と列サイズのマトリックスを指定できます。

以下の例では、デフォルトの`md`画面サイズ/ブレークポイントが使用されます：

```php
use BootstrapUI\View\Helper\FormHelper;

echo $this->Form->create($article, [
    'align' => [
        FormHelper::GRID_COLUMN_ONE => 4, // first column (span over 4 columns)
        FormHelper::GRID_COLUMN_TWO => 8, // second column (span over 8 columns)
    ],
]);
echo $this->Form->control('title');
echo $this->Form->control('published', ['type' => 'checkbox']);
echo $this->Form->end();
```

次の HTML がレンダリングされます:

```html
<form method="post" accept-charset="utf-8" class="form-horizontal" action="/articles/add">
    <!-- ... -->
    <div class="mb-3 form-group row text">
        <label class="col-form-label col-md-4" for="title">Title</label>
        <div class="col-md-8">
            <input type="text" name="title" id="title" class="form-control">
        </div>
    </div>
    <div class="mb-3 form-group row checkbox">
        <div class="offset-md-4 col-md-8">
            <div class="form-check">
                <input type="hidden" name="published" value="0"/>
                <input type="checkbox" name="published" value="1" id="published" class="form-check-input"/>
                <label class="form-check-label" for="published">Published</label>
            </div>
        </div>
    </div>
    <!-- ... -->
</form>
```

以下では、画面サイズ/ブレークポイントと列サイズのマトリックスを使用します。

```php
use BootstrapUI\View\Helper\FormHelper;

echo $this->Form->create($article, [
    'align' => [
        // column sizes for the `sm` screen-size/breakpoint
        'sm' => [
            FormHelper::GRID_COLUMN_ONE => 6,
            FormHelper::GRID_COLUMN_TWO => 6,
        ],
        // column sizes for the `md` screen-size/breakpoint
        'md' => [
            FormHelper::GRID_COLUMN_ONE => 4,
            FormHelper::GRID_COLUMN_TWO => 8,
        ],
    ],
]);
echo $this->Form->control('title');
echo $this->Form->control('published', ['type' => 'checkbox']);
echo $this->Form->end();
```

次の HTML がレンダリングされます:

```html
<form method="post" accept-charset="utf-8" class="form-horizontal" action="/articles/add">
    <!-- ... -->
    <div class="mb-3 form-group row text">
        <label class="col-form-label col-sm-6 col-md-4" for="title">Title</label>
        <div class="col-sm-6 col-md-8">
            <input type="text" name="title" id="title" class="form-control">
        </div>
    </div>
    <div class="mb-3 form-group row checkbox">
        <div class="offset-sm-6 offset-md-4 col-sm-6 col-md-8">
            <div class="form-check">
                <input type="hidden" name="published" value="0"/>
                <input type="checkbox" name="published" value="1" id="published" class="form-check-input"/>
                <label class="form-check-label" for="published">Published</label>
            </div>
        </div>
    </div>
    <!-- ... -->
</form>
```

デフォルトの配置では、`md` 画面サイズ/ブレークポイントと以下の列サイズが使用されます。

```php
[
    FormHelper::GRID_COLUMN_ONE => 2,
    FormHelper::GRID_COLUMN_TWO => 10,
]
```

### インラインフォーム(Inline forms)

インラインフォームでは、すべてのコントロールが同じ行にレンダリングされ、ほとんどのコントロールのラベルが非表示になります。

```php
echo $this->Form->create($article, [
    'align' => 'inline',
]);
echo $this->Form->control('title', ['placeholder' => 'Title']);
echo $this->Form->control('published', ['type' => 'checkbox']);
echo $this->Form->end();
```

次の HTML をレンダリングします:

```html
<form method="post" accept-charset="utf-8" class="form-inline" action="/articles/add">
    <!-- ... -->
    <div class="form-group text">
        <label class="form-label visually-hidden" for="title">Title</label>
        <input type="text" name="title" placeholder="Title" id="title" class="form-control"/>
    </div>
    <div class="form-check form-check-inline checkbox">
        <input type="hidden" name="published" value="0"/>
        <input type="checkbox" name="published" value="1" id="published" class="form-check-input">
        <label class="form-check-label" for="published">Published</label>
    </div>
    <!-- ... -->
</form>
```

### 間隔(Spacing)

BootstrapUIは、標準でフォームコントロールにいくつかのデフォルトの間隔を適用します。通常フォームおよび水平配置フォームには、すべてのコントロールに`mb-3` [spacing class](https://getbootstrap.com/docs/5.0/utilities/spacing/) 間隔クラスが適用され、インラインフォームには`g-3` [gutter class](https://getbootstrap.com/docs/5.0/layout/gutters/) ガタークラスが使用されます。

これは `spacing` オプションで変更可能です。すべての配置に対してヘルパー単位やフォーム単位で適用され、通常/水平配置の場合はコントロール単位でも適用されます。

```php
// for all forms
echo $this->Form->setConfig([
    'spacing' => 'mb-6',
]);
```

```php
// for a specific form
echo $this->Form->create($entity, [
    'spacing' => 'mb-6',
]);
```

```php
// for a specific control (default/horizontal aligned forms only)
echo $this->Form->control('title', [
    'spacing' => 'mb-6',
]);
```

この動作を完全に無効にするには、`spacing` オプションを `false` に設定してください。

### サポートされているコントロール(Supported controls)

BootstrapUIは、CakePHPのデフォルトコントロールすべてに対してBootstrap互換のマークアップをサポートおよび生成します。さらに、次のコントロールに対してBootstrap固有のマークアップも明示的にサポートしています：

- `color`
- `range`
- `switch`

### コンテナ属性(Container attributes)

外部コントロールコンテナの属性は、`container` オプションを使用して変更でき、単純な変更のためにカスタムテンプレートを使用する必要がなくなります。`class` 属性は特別なケースで、その値は既存のクラスリストを置き換えるのではなく、先頭に追加されます。

```php
echo $this->Form->control('title', [
    'container' => [
        'class' => 'my-title-control',
        'data-meta' => 'meta information',
    ],
]);
```

これにより、次の HTML が生成されます。

```html
<div data-meta="meta information" class="my-title-control mb-3 form-group text">
    <label class="form-label" for="title">Title</label>
    <input type="text" name="title" id="title" class="form-control">
</div>
```

### コンテンツの追加/先頭への挿入(Appending/Prepending content)

入力グループへのコンテンツの追加および先頭への挿入は、それぞれ `append` オプションと `prepend` オプションでサポートされています。

```php
echo $this->Form->control('email', [
    'prepend' => '@',
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-group email">
    <label class="form-label" for="email">Email</label>
    <div class="input-group">
        <span class="input-group-text">@</span>
        <input type="email" name="email" id="email" class="form-control"/>
    </div>
</div>
```

#### 複数のアドオン(Multiple addons)

複数のアドオンは、`append` および `prepend` オプションに配列として定義できます。

```php
echo $this->Form->control('amount', [
    'prepend' => ['$', '0.00'],
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-group text">
    <label class="form-label" for="amount">Amount</label>
    <div class="input-group">
        <span class="input-group-text">$</span>
        <span class="input-group-text">0.00</span>
        <input type="text" name="amount" id="amount" class="form-control"/>
    </div>
</div>
```

#### アドオンオプション(Addon options)

アドオンは、入力グループコンテナに適用されるオプションをサポートします。これらは、`append` および `prepend` オプションに配列を渡し、最後の要素としてオプションの配列を追加することで定義できます。

オプションには、コントロールオプションで使用されるHTML属性に加え、対応する入力グループのサイズクラスに自動変換される特別な `size` オプションを含めることができます。

```php
echo $this->Form->control('amount', [
    'prepend' => [
        '$',
        '0.00',
        [
            'size' => 'lg',
            'class' => 'custom',
            'custom' => 'attribute',
        ],
    ],
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-group text">
    <label class="form-label" for="amount">Amount</label>
    <div class="input-group input-group-lg custom" custom="attribute">
        <span class="input-group-text">$</span>
        <span class="input-group-text">0.00</span>
        <input type="text" name="amount" id="amount" class="form-control"/>
    </div>
</div>
```

### インラインチェックボックスとラジオボタン(Inline checkboxes and radio buttons)

[Inline checkboxes/switches and radio buttons](https://getbootstrap.com/docs/4.5/components/forms/#inline)は、inline オプションを true に設定することで作成できます。（インライン整列フォームと混同しないようにしてください）

インライン化されたチェックボックス/スイッチおよびラジオボタンは、同じ水平行にレンダリングされます。ただし、水平フォーム整列を使用する場合、同じ行にレンダリングされるのはマルチチェックボックスのみです！

```php
echo $this->Form->control('option_1', [
    'type' => 'checkbox',
    'inline' => true,
]);
echo $this->Form->control('option_2', [
    'type' => 'checkbox',
    'inline' => true,
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-check form-check-inline checkbox">
    <input type="hidden" name="option-1" value="0"/>
    <input type="checkbox" name="option-1" value="1" id="option-1" class="form-check-input">
    <label class="form-check-label" for="option-1">Option 1</label>
</div>
<div class="mb-3 form-check form-check-inline checkbox">
    <input type="hidden" name="option-2" value="0"/>
    <input type="checkbox" name="option-2" value="2" id="option-2" class="form-check-input">
    <label class="form-check-label" for="option-2">Option 2</label>
</div>
```

### スイッチ(Switches)

[Switch style checkboxes](https://getbootstrap.com/docs/5.0/forms/checks-radios/#switches)は、switch オプションを true に設定することで作成できます。

```php
echo $this->Form->control('option', [
    'type' => 'checkbox',
    'switch' => true,
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-group form-check form-switch checkbox">
    <input type="hidden" name="option" value="0"/>
    <input type="checkbox" name="option" value="1" id="option" class="form-check-input">
    <label class="form-check-label" for="option">Option</label>
</div>
```

### フローティングラベル(Floating labels)

[Floating labels](https://getbootstrap.com/docs/5.0/forms/floating-labels)は、`text`, `textarea`、および（`multiple` ではない）`select` コントロールでサポートされています。ラベルの `floating` オプションを有効にすることで使用できます：

```php
echo $this->Form->control('title', [
    'label' => [
        'floating' => true,
    ],
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-floating form-group text">
    <input type="text" name="title" id="title" class="form-control"/>
    <label for="title">Title</label>
</div>
```

### ヘルプテキスト(Help text)

Bootstrapの[form help text](https://getbootstrap.com/docs/4.5/components/forms/#help-text)は、help オプションでサポートされています。

ヘルプテキストはデフォルトで、コントロールとバリデーションフィードバックの間にレンダリングされます。

```php
echo $this->Form->control('title', [
    'help' => 'Help text',
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-group text">
    <label class="form-label" for="title">Title</label>
    <input type="text" name="title" id="title" class="form-control" aria-describedby="title-help"/>
    <small id="title-help" class="d-block form-text text-muted">Help text</small>
</div>
```

属性は、`help` オプションに配列を渡して設定でき、テキストは `content` キーで定義します。

```php
echo $this->Form->control('title', [
    'help' => [
        'id' => 'custom-help',
        'class' => 'custom',
        'data-custom' => 'attribute',
        'content' => 'Help text',
    ],
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-group text">
    <label class="form-label" for="title">Title</label>
    <input type="text" name="title" id="title" class="form-control" aria-describedby="custom-help"/>
    <small id="custom-help" class="custom d-block form-text text-muted" data-custom="attribute">Help text</small>
</div>
```

### ツールチップ(Tooltips)

[Bootstrap tooltips](https://getbootstrap.com/docs/5.0/components/tooltips/)は、`tooltip` オプションを使用してラベルに追加できます。ツールチップのトグルは、デフォルトで[Bootstrap icon](https://icons.getbootstrap.com/)としてレンダリングされ、`install` コマンドでアセットをインストールすると自動的に含まれます。

```php
echo $this->Form->control('title', [
    'tooltip' => 'Tooltip text',
]);
```

これにより、次の HTML が生成されます。

```html
<div class="mb-3 form-group text">
    <label class="form-label" for="title">
        Title <span data-bs-toggle="tooltip" title="Tooltip text" class="bi bi-info-circle-fill"></span>
    </label>
    <input type="text" name="title" id="title" class="form-control"/>
</div>
```

別のトグル（別のBootstrapアイコンやまったく異なるアイコンフォント/ライブラリ）を使用したい場合は、[overriding the `tooltip` template](https://book.cakephp.org/4/en/views/helpers/form.html#customizing-the-templates-formhelper-uses)することで対応できます。これは、グローバル、フォーム単位、またはコントロール単位で設定できます。

```php
echo $this->Form->control('title', [
    'tooltip' => 'Tooltip text',
    'templates' => [
        'tooltip' => '<span data-bs-toggle="tooltip" title="{{content}}" class="material-icons">info</span>',
    ],
]);
```

### エラーフィードバックスタイル(Error feedback style)

BootstrapUIは、[regular Bootstrap text feedback](https://getbootstrap.com/docs/4.5/components/forms/#validation)と、[Bootstrap tooltip feedback](https://getbootstrap.com/docs/4.5/components/forms/#tooltips)の2つのエラーフィードバックスタイルをサポートしています（tooltip オプションで設定されるラベルのツールチップとは混同しないでください！）。

スタイルは、`feedbackStyle` オプションでグローバル、フォーム単位、またはコントロール単位で設定できます。サポートされているスタイルは次の通りです：

- `\BootstrapUI\View\Helper\FormHelper::FEEDBACK_STYLE_DEFAULT` 通常のBootstrapテキストフィードバックとしてエラーフィードバックをレンダリングします。
- `\BootstrapUI\View\Helper\FormHelper::FEEDBACK_STYLE_TOOLTIP` Bootstrapツールチップフィードバックとしてエラーフィードバックをレンダリングします（インラインフォームではこのスタイルがデフォルトで使用されます）。

ツールチップエラースタイルを使用する場合、フォームグループ要素は静的配置でない必要がある点に注意してください！フォームヘルパーは、ツールチップエラースタイルが有効な場合、自動的にBootstrapの[position utility class](https://getbootstrap.com/docs/4.5/utilities/position/) `position-relative` をフォームグループ要素に追加します。

異なる配置が必要な場合は、`.form-group` 要素の `position` ルールをCSSで上書きするか、`formGroupPosition` オプションを使用して、グローバル、フォーム単位、またはコントロール単位で希望の位置を設定してください。サポートされている値は以下の通りです：

- `\BootstrapUI\View\Helper\FormHelper::POSITION_ABSOLUTE`
- `\BootstrapUI\View\Helper\FormHelper::POSITION_FIXED`
- `\BootstrapUI\View\Helper\FormHelper::POSITION_RELATIVE`
- `\BootstrapUI\View\Helper\FormHelper::POSITION_STATIC`
- `\BootstrapUI\View\Helper\FormHelper::POSITION_STICKY`

```php
$this->Form->setConfig([
    'feedbackStyle' => \BootstrapUI\View\Helper\FormHelper::FEEDBACK_STYLE_TOOLTIP,
    'formGroupPosition' => \BootstrapUI\View\Helper\FormHelper::POSITION_ABSOLUTE,
]);

// ...

echo $this->Form->control('title');
```

`title` フィールドにエラーがある場合、次の HTML が生成されます。

```html
<div class="mb-3 form-group position-absolute text is-invalid">
    <label class="form-label" for="title">Title</label>
    <input type="text" name="title" id="title" class="is-invalid form-control"/>
    <div class="invalid-tooltip">Error message</div>
</div>
```

### フラッシュメッセージ/アラート(Flash Messages / Alerts)

Flashメッセージは、デフォルトのFlashコンポーネントの構文で設定できます。サポートされているタイプは `success`, `info`, `warning`,`error` です。

```php
$this->Flash->success('Your Success Message.');
```

#### アラートスタイル(Alert styles)

他のBootstrapのアラートスタイルを設定する場合は、次のように行います。

```php
$this->Flash->set('Your Dark Message.', ['params' => ['class' => 'dark']]);
```

サポートされているスタイルは、`primary`、`secondary`、`light`、`dark` です。

#### アイコン(Icons)

デフォルトでは、アラートはタイプに応じたBootstrapアイコンを使用します。対応するタイプは、`default`, `info`, `warning`,`error`, `success` です。アイコンは、`icon` オプション/パラメータで無効化またはカスタマイズでき、Flashヘルパー全体に適用するか、個別のメッセージに対して設定できます。

アイコンなしのメッセージ：

```php
$this->Flash->success('Message without icon.', [
    'params' => [
        'icon' => false,
    ],
]);
```

カスタムアイコンを使用する:

```php
$this->Flash->success('Message with custom icon.', [
    'params' => [
        'icon' => 'mic-mute-fill',
    ],
]);
```

アイコンオプションを渡します（ここでアイコン名は任意です。省略された場合はデフォルトのアイコンマップが参照されます）：

```php
$this->Flash->success('Message with custom icon options.', [
    'params' => [
        'icon' => [
            'name' => 'mic-mute-fill',
            'size' => '2xl',
            'class' => 'foo bar me-2',
            'data-custom' => 'attribute',
        ],
    ],
]);
```

```html
<i class="foo bar me-2 bi bi-mic-mute-fill bi-2xl" data-custom="attribute"></i>
```

カスタム HTML を使用する:

```php
$this->Flash->success('Message with custom icon HTML.', [
    'params' => [
        'icon' => '<span class="material-icons">volume_off</span>',
    ],
]);
```

すべてのFlashメッセージのアイコンを無効にする：

```php
$this->loadHelper('Flash', [
    'className' => 'BootstrapUI.Flash',
    'icon' => false,
]);
```

すべてのFlashメッセージに対してアイコンオプションを設定する（デフォルトのアイコンマップが使用され、オプションはすべてのアイコンに適用されます）：

```php
$this->loadHelper('Flash', [
    'className' => 'BootstrapUI.Flash',
    'icon' => [
        'size' => '2xl',
        'class' => 'foo bar me-2',
        'data-custom' => 'attribute',
    ],
]);
```

カスタムアイコンマップを定義する：

```php
$this->loadHelper('Flash', [
    'className' => 'BootstrapUI.Flash',
    'iconMap' => [
        'default' => 'info-circle-fill',
        'success' => 'check-circle-fill',
        'error' => 'exclamation-triangle-fill',
        'info' => 'info-circle-fill',
        'warning' => 'exclamation-triangle-fill',
    ],
]);
```

別のアイコンセットを使用する：

```php
$this->Flash->success('Message with different icon set.', [
    'params' => [
        'icon' => [
            'namespace' => 'fas',
            'prefix' => 'fa',
            'name' => 'microphone-slash',
            'size' => '2xl',
        ],
    ],
]);
```

```html
<i class="me-2 fas fa-microphone-slash fa-2xl"></i>
```

すべてのFlashメッセージに対して別のアイコンセットを使用する：

```php
$this->loadHelper('Html', [
    'className' => 'BootstrapUI.Html',
    'iconDefaults' => [
        'namespace' => 'fas',
        'prefix' => 'fa',
    ],
]);
```

```php
$this->loadHelper('Flash', [
    'className' => 'BootstrapUI.Flash',
    'iconMap' => [
        'default' => 'info-circle',
        'success' => 'check-circle',
        'error' => 'exclamation-triangle',
        'info' => 'info-circle',
        'warning' => 'exclamation-triangle',
    ],
]);
```

### バッジ(Badges)

デフォルトでは、バッジは `secondary` テーマスタイルでレンダリングされます：

```php
echo $this->Html->badge('Text');
```

```html
<span class="badge bg-secondary">Text</span>
```

#### 背景色(Background colors)

[Background colors](https://getbootstrap.com/docs/5.0/components/badge/#background-colors)は、class オプションでBootstrapのテーマカラー名を指定することで変更できます。ヘルパーは、正しいプレフィックスが適用されるように処理します：

```php
echo $this->Html->badge('Text', [
    'class' => 'danger',
]);
```

```html
<span class="badge bg-danger">Text</span>
```

#### 別のHTMLタグを使用する(Using a different HTML tag)

デフォルトでは、バッジは `<span>` タグを使用します。これは、`tag` オプションで変更できます：

```php
echo $this->Html->badge('Text', [
    'tag' => 'div',
]);
```

```html
<div class="badge bg-secondary">Text</div>
```

### アイコン(Icons)

デフォルトでHTMLヘルパーは、[Bootstrap icons](https://icons.getbootstrap.com/)を使用するように設定されています。

```php
echo $this->Html->icon('mic-mute-fill');
```

```html
<i class="bi bi-mic-mute-fill"></i>
```

#### サイズ(Sizes)

サイズは `size` オプションで指定でき、渡された値には自動的にプレフィックスが付与されます：

```php
echo $this->Html->icon('mic-mute-fill', [
    'size' => '2xl',
]);
```

```html
<i class="bi bi-mic-mute-fill bi-2xl"></i>
```

このプラグインには、アイコンを垂直中央に揃えるサイズとして `2xs`, `xs`,`sm`, `lg`, `xl`, `2xl` が含まれています。また、アイコンをベースラインに揃えるサイズとして `1x`, `2x`, `3x`, `4x`, `5x`,`6x`, `7x`, `8x`, `9x`, `10x` も提供されています。

#### 別のアイコンセットを使用する(Using a different icon set)

別のアイコンセットを使用するには、`icon()` 呼び出しごとに `namespace` および `prefix` オプションを設定します：

```php
echo $this->Html->icon('microphone-slash', [
    'namespace' => 'fas',
    'prefix' => 'fa',
]);
```

または、HTMLヘルパーのデフォルトを設定することで、HtmlHelper::icon() のすべての使用に対してグローバルに適用できます：

```php
$this->loadHelper('Html', [
    'className' => 'BootstrapUI.Html',
    'iconDefaults' => [
        'namespace' => 'fas',
        'prefix' => 'fa',
    ],
]);
```

### パンくずリスト(Breadcrumbs)

Breadcrumbsヘルパーはそのまま置き換えて使用でき、追加の設定は不要です。

```php
echo $this->Breadcrumbs
    ->add('Home', '/')
    ->add('Articles', '/articles')
    ->add('View')
    ->render();
```

```html
<nav aria-label="breadcrumb">
    <ol class="breadcrumb">
        <li class="breadcrumb-item"><a href="/">Home</a></li>
        <li class="breadcrumb-item active"><a href="/articles" aria-current="page">Articles</a></li>
        <li class="breadcrumb-item"><span>View</span></li>
    </ol>
</nav>
```

### ページネーション(Pagination)

Paginatorヘルパーは、標準メソッドを使用する際にBootstrap互換のスタイル付きマークアップを生成します。また、先頭/前/次/最後のリンクやページ番号リンクを含む完全なページネーションコントロールを、リストラッパー内にまとめて生成する便利なメソッドも提供しています。

```php
echo $this->Paginator->first();
echo $this->Paginator->prev();
echo $this->Paginator->numbers();
echo $this->Paginator->next();
echo $this->Paginator->last();
```

これにより、次の HTML が生成されます。

```html
<li class="page-item first">
    <a class="page-link" aria-label="First" href="/articles/index">
        <span aria-hidden="true">«</span>
    </a>
</li>
<li class="page-item">
    <a class="page-link" rel="prev" aria-label="Previous" href="/articles/index">
        <span aria-hidden="true">‹</span>
    </a>
</li>
<li class="page-item">
    <a class="page-link" href="/articles/index">1</a>
</li>
<li class="page-item active" aria-current="page">
    <a class="page-link" href="#">2</a>
</li>
<li class="page-item">
    <a class="page-link" href="/articles/index?page=3">3</a>
</li>
<li class="page-item">
    <a class="page-link" rel="next" aria-label="Next" href="/articles/index?page=3">
        <span aria-hidden="true">›</span>
    </a>
</li>
<li class="page-item last">
    <a class="page-link" aria-label="Last" href="/articles/index?page=3">
        <span aria-hidden="true">»</span>
    </a>
</li>
```

#### ARIAラベルの設定(Configuring the ARIA labels)

標準メソッドを使用する場合、label オプションでカスタム文字列を渡し、[the `aria-label` attribute](https://getbootstrap.com/docs/5.0/components/pagination/#working-with-icons)に使用できます：

```php
echo $this->Paginator->first('«', ['label' => __('Beginning')]);
echo $this->Paginator->prev('‹', ['label' => __('Back')]);
echo $this->Paginator->next('›', ['label' => __('Forward')]);
echo $this->Paginator->last('»', ['label' => __('End')]);
```

これにより、次のHTMLが生成されます：

```html
<li class="page-item first">
    <a class="page-link" aria-label="Beginning" href="/articles/index">
        <span aria-hidden="true">«</span>
    </a>
</li>
<li class="page-item">
    <a class="page-link" rel="prev" aria-label="Back" href="/articles/index">
        <span aria-hidden="true">‹</span>
    </a>
</li>
<li class="page-item">
    <a class="page-link" rel="next" aria-label="Forward" href="/articles/index?page=3">
        <span aria-hidden="true">›</span>
    </a>
</li>
<li class="page-item last">
    <a class="page-link" aria-label="End" href="/articles/index?page=3">
        <span aria-hidden="true">»</span>
    </a>
</li>
```

#### 完全なコントロールセットを生成する(Generating a full set of controls)

先頭/前/次/最後のリンクやページ番号リンクを含む完全なページネーションコントロールセットは、`links()` メソッドを使用してリストラッパー内にまとめて生成できます。

By default it renders numbers only:

```php
echo $this->Paginator->links();
```

デフォルトでは、ページ番号のみがレンダリングされます：

```html
<ul class="pagination">
    <li class="page-item">
        <a class="page-link" href="/articles/index">1</a>
    </li>
    <li class="page-item active" aria-current="page">
        <a class="page-link" href="#">2</a>
    </li>
    <li class="page-item">
        <a class="page-link" href="/articles/index?page=3">3</a>
    </li>
</ul>
```

##### コントロールの設定(Configuring controls)

生成されるコントロールは、`first`, `prev`, `next`, `last` オプションで設定できます。それぞれ、`true` を指定するとヘルパーのデフォルトで生成され、文字列を指定するとコントロールのテキストとして使用されます。また、`label` と `text` オプションを含む配列を使用して、ARIAラベルの値とリンクテキストを指定することもできます：

```php
echo $this->Paginator->links([
    'first' => '❮❮',
    'prev' => true,
    'next' => true,
    'last' => [
        'label' => 'End',
        'text' => '❯❯',
    ],
]);
```

これにより、次のHTMLが生成されます：

```html
<ul class="pagination">
    <li class="page-item first">
        <a class="page-link" aria-label="First" href="/articles/index">
            <span aria-hidden="true">❮❮</span>
        </a>
    </li>
    <li class="page-item">
        <a class="page-link" rel="prev" aria-label="Previous" href="/articles/index">
            <span aria-hidden="true">‹</span>
        </a>
    </li>
    <li class="page-item">
        <a class="page-link" href="/articles/index">1</a>
    </li>
    <li class="page-item active" aria-current="page">
        <a class="page-link" href="#">2</a>
    </li>
    <li class="page-item">
        <a class="page-link" href="/articles/index?page=3">3</a>
    </li>
    <li class="page-item">
        <a class="page-link" rel="next" aria-label="Next" href="/articles/index?page=3">
            <span aria-hidden="true">›</span>
        </a>
    </li>
    <li class="page-item last">
        <a class="page-link" aria-label="End" href="/articles/index?page=3">
            <span aria-hidden="true">❯❯</span>
        </a>
    </li>
</ul>
```

##### サイジング(Sizing)

[The size](https://getbootstrap.com/docs/5.0/components/pagination/#sizing)は、`size` オプションで指定できます：

```php
echo $this->Paginator->links([
    'size' => 'lg',
]);
```

これにより、次のHTMLが生成されます：

```html
<ul class="pagination pagination-lg">
    <!-- ... -->
</ul>
```

### ヘルパーの設定(Helper configuration)

各ヘルパーは、`AppView.php` で読み込む際に追加のパラメータを渡すことで設定できます。

以下は、Paginatorヘルパーの `prev` および `next` ラベルを変更する例です。

```php
$this->loadHelper('Paginator', [
    'className' => 'BootstrapUI.Paginator',
    'labels' => [
        'prev' => 'previous',
        'next' => 'next',
    ],
]);
```

## 貢献方法(Contributing)

### パッチと機能追加(Patches & Features)

* フォーク(Fork)
* 修正、改良(Mod, fix)
* テスト(Test) - 意図しない不具合を防ぐために重要です
* コミット(Commit) - ライセンス、TODO、バージョンなどを変更しないでください（変更する場合は、プル時に無視できるように別のコミットに分けてください）
* プルリクエスト(Pull request) - トピックブランチを使うと高評価です

上流にプルリクエスト（PR）が採用されるようにするため、CakePHPのコーディング標準に従う必要があります。`pre-commit` フックが含まれており、コードスニッフを自動的に実行します。プロジェクトのルートディレクトリから以下を実行してください：

```
cp ./contrib/pre-commit .git/hooks/
chmod 755 .git/hooks/pre-commit
```

### テスト(Testing)

プラグインのコードを作業する際、以下の手順でBootstrapUIのテストを実行できます：

```
composer install
./vendor/bin/phpunit
```

### バグとフィードバック(Bugs & Feedback)

https://github.com/friendsofcake/bootstrap-ui/issues


[cakephp]:https://cakephp.org/
[composer]:https://getcomposer.org/
[composer:ignore]:https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md
[bs]:https://getbootstrap.com/
[npm]:https://www.npmjs.com/