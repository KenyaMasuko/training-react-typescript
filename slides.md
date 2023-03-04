---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
titleTemplate: "React研修資料 - ③"
exportFilename: "React研修資料 - ③"
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

# React 研修資料 ③

<div class="pt-12">
  <span  class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

---

# 目次

1. Next.js とは
1. Next.js ハンズオン（環境構築）
1. Next.js の特徴
1. SSR,SSG,ISR とは
1. Next.js と HeadlessCMS でブログを作成する

---

# 1.1 Next.js とは

<div>

[この資料](https://speakerdeck.com/recruitengineers/nextjs-2022?slide=27)で進めます
<br />
<br />

みる内容

1. Next.js の特徴
1. 3 つの両極

</div>

---

# 2.1 Next.js ハンズオン（環境構築）

<div>
環境構築の手順

1. Next.js で環境構築したいディレクトリまで移動する

2. terminal で下記のコマンドを実行

```shell
npx create-next-app@latest nextjs-blog --use-npm --exmaple '参考にしたいリポジトリ'
```

参考 repo: https://github.com/vercel/next-learn/tree/master/basics/learn-starter

3. 質問に答える

```shell
Need to install the following packages:
  create-next-app@13.1.6
Ok to proceed? (y) y
✔ Would you like to use TypeScript with this project? … Yes
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use `src/` directory with this project? … Yes
✔ Would you like to use experimental `app/` directory with this project? … No
✔ What import alias would you like configured? … @/*
```

</div>

---

# 2.2

---

# 3.1 Next.js の特徴

<div>

1. ファイルシステムベースのルーティング
1. リンク遷移
1. 画像最適化
1. meta 情報
1. CSS Modules
1. SG(SSG)と SSR
1. getStaticProps と getServerSideProps
1. dynamic routes

</div>

Node.js で環境構築

1. src ディレクトリを作成して、index.ts ファイルを作成する
1. npm scripts を package.json に追加する
   ```json
   "scripts": {
   "dev": "nodemon --watch \"src/**/*.ts\" --exec ts-node --files ./src/index.ts"
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

   `name?: string`と`nama: string | undefined`を区別できるオプション。プロパティを省略したのか書き忘れたのかの違いを明確にするために必要。
