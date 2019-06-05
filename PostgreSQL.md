# PostgreSQL

## PostgreSQLの起動

```psql
psql -U ユーザ名
```

## データベースの一覧表示

```psql
\l
```

## データベースの選択

```psql
\c データベース名
```

## テーブルの一覧表示

```psql
\d
```

## テーブルの詳細表示

```psql
\d 表名
```

## PostgreSQLの停止

```psql
\q
```

## 作業ディレクトリの移動

```psql
\cd ディレクトリ名
```

## SQLファイルのインポート

```psql
\i SQLファイル名
```

## NULL表示

```psql
\pset null NULL
```

## 時間計測の有効化

```psql
\timing
```

## 自動コミットの無効化

```psql
\set AUTOCOMMIT off
```

## 環境変数の設定値表示

```psql
\echo :環境変数名
```

## スキーマの一覧表示

```psql
\z
```