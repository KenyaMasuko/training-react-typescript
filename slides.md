---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
titleTemplate: "React研修資料 - ⓪"
exportFilename: "React研修資料 - ⓪"
# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: prism
# show line numbers in code blocks
lineNumbers: false
# persist drawings in exports and build
drawings:
  persist: true
# use UnoCSS
css: unocss
---

# React 研修資料 ⓪

<div class="pt-12">
  <span  class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

---

# 環境構築 ~ TypeScript 編 ~

<div>
今後の研修は全てTypeScriptを使用して行なっていきます。

基本的な TypeScript の研修は<a href="https://drive.google.com/file/d/14XcWt4SamRaUts4stDpYms41eVzpgmqe/view" target="_blank">こちら</a>の資料を使用します。

</div>

---

# Node.js をインストールする

Node.js で環境構築

<div>

### <a href="https://nodejs.org/ja/about/" target="_blank">Node.js</a> とは

> Node.js はスケーラブルなネットワークアプリケーションを構築するために設計された非同期型のイベント駆動の JavaScript 環境です。

Node.js をインストールする

1. インストーラー経由でインストールする

   [公式インストーラー](https://nodejs.org/ja/download/)

2. バージョンマネージャーを使用する（おすすめ）
   1. [volta](https://volta.sh/)
   2. [nvm](https://github.com/nvm-sh/nvm)

</div>

---

# 必要なパッケージをインストールする

Node.js で環境構築

```shell
$ yarn init -y

$ yarn add -D typescript ts-node @types/node nodemon

$ npx tsc --init

$ yarn add -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin

```

---

# 必要なパッケージングをインストールする

Node.js で環境構築

```shell
$ npx eslint --init
You can also run this command directly using 'npm init @eslint/config'.
Need to install the following packages:
  @eslint/create-config
Ok to proceed? (y) y
✔ How would you like to use ESLint? · To check syntax and find problems
✔ What type of modules does your project use? · JavaScript modules (import/export)
✔ Which framework does your project use? · None of these
✔ Does your project use TypeScript? · Yes
✔ Where does your code run? · browser
✔ What format do you want your config file to be in? · JSON
The config that you've selected requires the following dependencies:

@typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
✔ Would you like to install them now? · Yes
✔ Which package manager do you want to use? · yarn

$ yarn add -D prettier eslint-config-prettier

$ touch .prettierrc.json

```

---

# 初期設定

Node.js で環境構築

1. src ディレクトリを作成して、index.ts ファイルを作成する
1. npm scripts を package.json に追加する
   ```json
   "scripts": {
   "dev": "nodemon --watch \"src/**/*.ts\" --exec ts-node-esm --files ./src/index.ts"
   },
   ```
1. package.json に`"type": "module"`を設定する
1. `yarn dev`で Node.js を動かす
1. ファイルをセーブした際に index.ts に入力したプログラムが実行されれば OK

---

# tsconfig.json の設定

<div>
tsconfig.jsonはTypeScriptのコンパイルに関する設定ファイルです。<br>
このファイルはTypeScriptコンパイラ（tsc）が自動で生成してくれます。TypeScriptの環境構築の流れをざっくりまとめると下記のようになります。
<div class="h-5"></div>

1. `npm init`で package.json を生成する
2. `npm install -D typescript`で TypeScript コンパイラを node_modules 内にインストールする
3. `npx tsc --init`で tsconfig.json を生成する（node_modules の中にインストールした tsc コマンドが使われる）
4. 生成された tsconfig.json を必要に応じて編集する

</div>

---

# 基本的な設定 ①

tsconfig.json の設定

1. `"target": "es2020"`

   target コンパイルオプションはトランスパイルの程度を指定します。デフォルトの es2016 では ES2016 以下の構文のみ解釈できる環境でも動作するように、それよりも新しい構文はトランスパイルするという意味です。

2. `"module": "esnext"`

   これはモジュールに関係の構文をどう扱うかを決めるオプションです。古いバージョンの Node.js では CommonJS（CJS）形式のモジュールしか扱えませんでしたが、現在の Node.js のバージョンでは ES Modules（ESM）形式でモジュールを扱えるのでこのコンパイラオプションを追加します。

---

# 基本的な設定 ②

tsconfig.json の設定

3. `"moduleResolution": "node"`

   npm でインストールしたモジュールを利用できるようにするためのコンパイラオプション。

4. `"outDir": "./dist"`

   コンパイル後のファイル出力先を指定する。一般にコンパイル後のファイルは Github にはあげないので、dist 配下は gitignore に追加記載する。

---

# ファイル周りのオプション ①

tsconfig.json の設定

```json
{
	"include": ["./src/**/*.ts"]
}
```

上記の設定の意味を見ていきます。

1. include では tsc のコンパイル対象のファイルを設定する
2. 配列形式で記述できる

   ```json
   {
   	"include": ["./src/**/*.ts", "./lib/**/*.ts"]
   }
   ```

3. いわゆる`glob`形式が使える

   `**/`は 0 個以上のディレクトリ。`*`は任意のファイル名を表す。

---

# ファイル周りのオプション ②

tsconfig.json の設定

4. exclude の設定

   include で指定した中のコンパイル対象にしたくないファイル（テストコードなど）を除外する

   ```json
   {
   	"include": ["./src/**/*.ts"],
   	"exclude": ["./src/__tests__/**/*.ts"]
   }
   ```

---

# チェックの厳格さに関するオプション ①

tsconfig.json の設定

1. `"strict": "true"`

   このオプションが 1 番重要。型安全に開発をするために必要なオプションをまとめてオンにできる。今後さらに型チェックを厳格にするオプションがつかされても、`"strict":"true"`にしておくだけで事足りる可能性が高い。特別な事情を除いて基本的にはオンにする。

2. `strictNullChecks`

   この設定を`true`にしないと tsc が`null`型と`undefined`型をチェックしません（`strict`を true にしておけば大丈夫です）。null の存在をちゃんと型システムで扱える →`null安全`なシステム

3. `noImplicitAny`

   このオプションが効果を発揮するのは、「関数宣言における引数の型宣言の取り扱い」です。詳しくは TypeScript の研修時にも説明しますが、このオプションを true にした例を記載します。

   ```ts
   const f = (num) => num * 2; // Parameter 'num' implicit has an 'any' type
   ```

---

# チェックの厳格さに関するオプション ②（発展）

tsconfig.json の設定

4. `noUncheckedIndexedAccess`

   TypeScript はインデックスアクセスを利用して、データにアクセスした際に、そのデータが undefined である可能性をチェックできない。その穴を埋めるのがこのオプション。

5. `exactOptionalPropertyTypes`

   `name?: string`と`name: string | undefined`を区別できるオプション。プロパティを省略したのか書き忘れたのかの違いを明確にするために必要。
