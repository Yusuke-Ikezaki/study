# SQL

## データ操作文（DML）

### SELECT文

#### 全表検索

```sql
SELECT * FROM 表名;
```

#### 特定列検索

```sql
SELECT 列名[, 列名...] FROM 表名;
```

##### ORDER BY句

```sql
# 昇順（デフォルト）
SELECT 列名 FROM 表名 ORDER BY 列名;

# 降順
SELECT 列名 FROM 表名 ORDER BY 列名 DESC;
```

#### 特定行検索

##### WHERE句

```sql
SELECT * FROM 表名 WHERE 条件式;
```

検索条件式

- 比較演算子

|比較演算記号|意味|
|:--|:--|
|= 値1|値1と等しい|
|>= 値1|値1以上|
|> 値1|値1より大きい|
|<= 値1|値1以下|
|< 値1|値1より小さい|
|<> 値1|値1と等しくない|
|IN (値1, 値2, ...)|リストの中の値と一致する|
|BETWEEN 値1 AND 値2|値1と値2の間にある|
|IS NULL / IS NOT NULL|列の値がNULLである / NULLでない|

- あいまい検索

|文字列演算子|意味|
|:--|:--|
|LIKE '値'|データの中に'値'の文字列を含む|

|ワイルドカード|意味|
|:--|:--|
|%|任意の文字列|
|_|任意の1文字|

- 論理演算子

```sql
# すべての条件式を満たす場合に真
条件式 AND 条件式

# いずれかの条件式を満たす場合に真
条件式 OR 条件式

# 条件式を満たさない場合に真
NOT 条件式
```

優先順位は

1. 比較演算子
1. NOT演算子
1. AND演算子
1. OR演算子

#### 特定フィールド検索

```sql
SELECT 列名 FROM 表名 WHERE 条件式;
```

#### 列名の編集

```sql
SELECT 列名 AS 別名 FROM 表名;
```

#### 計算結果を出力する

- 四則演算

```sql
SELECT 列名 演算子 数値 FROM 表名;
```

- 数値計算用関数

```sql
SELECT 関数名(引数) FROM 表名;
```

|関数名|意味|
|:--|:--|
|ABS(x)|xの絶対値|
|MOD(x, y)|xをyで割った余り|
|TRUNC(x, y)|xの小数点以下y桁を切り捨て|

- 文字列操作関数

```sql
SELECT 関数名(引数) FROM 表名;
```

|関数名|意味|
|:--|:--|
|CONCAT(str1, ...)|指定した文字列を結合する|
|SUBSTRING(str, x, y)|文字列strの先頭からx番目の文字からy文字取り出す|
|REPLACE(str, str1, str2)|str内のstr1の部分をstr2に置き換える|
|UPPER(str) / LOWER(str)|strを大文字に直す / 小文字に直す|
|TRIM(str1 FROM str2)|str2からstr1を取り除く|
|RPAD(str1, len, str2) / LPAD(str1, len, str2)|str1が文字数lenになるまで右側 / 左側からstr2を埋めた文字列を作る|

- 日付データ操作関数

```sql
SELECT 関数名(引数) FROM 表名;
```

|関数名|意味|
|:--|:--|
|DAY(date) / MONTH(data)|日付データの日 / 月の値を返す|

#### NULLの扱い

- 列に値が入っていない状態
- NULL値に対する演算の結果はNULL
- NULLを扱う演算子は`IS NULL`
- NULLを扱う関数は`COALESCE(式, ...)`

#### 行のグループ化

##### GROUP BY句

```sql
SELECT 列名 FROM 表名 GROUP BY 列名;
```

#### グループ関数

```sql
SELECT 関数名(列名) FROM 表名 GROUP BY 列名;
```

|関数名|意味|
|:--|:--|
|AVG([DISTINCT \| ALL] 式)|グループ内の列データ式の平均値を計算する|
|COUNT(*)|グループ内の全行の数を計算する|
|COUNT([DISTINCT \| ALL] 式)|NULL値にならない列データ式の値の個数を計算する|
|SUM([DISTINCT \| ALL] 式)|NULL値にならない列データ式の値の合計値を計算する|
|MAX([DISTINCT \| ALL] 式)|NULL値にならない列データ式の最大値|
|MIN([DISTINCT \| ALL] 式)|NULL値にならない列データ式の最小値|
|STDDEV([ALL] 式)|標準偏差を計算する|
|VARIANCE([ALL] 式)|分散を計算する|

#### グループ化とHAVING句

```sql
SELECT 列名 FROM 表名 GROUP BY 列名 HAVING 条件;
```

#### グループ化とWHERE句

```sql
SELECT 列名 FROM 表名 WHERE 条件 GROUP BY 列名;
```

#### 表の結合

```sql
SELECT [表名.]列名 
FROM 表名1 [LEFT | RIGHT] JOIN 表名2
ON 表名1.列名 = 表名2.列名
WHERE 条件式
```

#### 副問い合わせ

##### 単一行副問い合わせ

```sql
SELECT 列名 FROM 表名 
    WHERE 列名 比較演算子 (SELECT 関数(引数) FROM 表名)
```

##### 複数行副問い合わせ

- いずれかと値が一致する行の検索

```sql
SELECT 列名 FROM 表名 
    WHERE 列名 IN (SELECT 列名 FROM 表名 WHERE 条件)
```

- いずれかの値に対して条件式が真になる行の検索

```sql
SELECT 列名 FROM 表名 
    WHERE 列名 比較演算子 ANY (SELECT 列名 FROM 表名 WHERE 条件)
```

- すべての値に対して条件式が真になる行の検索

```sql
SELECT 列名 FROM 表名 
    WHERE 列名 比較演算子 ALL (SELECT 列名 FROM 表名 WHERE 条件)
```

### INSERT文

#### レコードの追加

```sql
# 表の全列の値を指定する場合
INSERT INTO 表名 VALUES(値1, 値2, ...);

# 表の特定の列を指定する場合
INSERT INTO 表名(列名1, 列名2, ...) VALUES(値1, 値2, ...)
```

#### 表の結合（MERGE）

```sql
# 表の全列の値を指定する場合
INSERT INTO 表名 副問い合わせ文

# 表の特定の列を指定する場合
INSERT INTO 表名(列1, 列2, ...) 副問い合わせ文
```

### UPDATE文

#### データの変更

```sql
UPDATE 表名 SET 列名1 = 値1, 列名2 = 値2, ... WHERE 条件式;
```

#### 副問い合わせを利用した変更

```sql
UPDATE 表名 SET 列名 = (副問い合わせ文) 
    WHERE 列名 = (副問い合わせ文)
```

### DELETE文

#### レコードの削除

```sql
DELETE FROM 表名 WHERE 条件式;
```

#### 副問い合わせを利用した削除

```sql
DELETE FROM 表名 WHERE 列名 = (副問い合わせ文);
```

## データ定義文（DDL）

### 表

#### 表の作成

```sql
# 列の情報を指定する
CREATE TABLE 表名 (
    列名1 データ型1 属性1 属性2 ... 
    [, 列名2 データ型2 属性 ...]
);

# 副問い合わせを利用する
CREATE TABLE 表名 AS 副問い合わせ文;
```

|データ型|意味|値の範囲|
|:--|:--|:--|
|INTEGER|整数|-2147483648 ~ 2147483627|
|FLAOT / REAL|単精度浮動小数点|-3.203823466E38 ~ -1.175494351E-38|
|DOUBLE / PRECISION|倍精度浮動小数点|-1.7976931348623E308 ~ -2.2250738585072014E-308|
|CHAR(n)|固定長の文字列|nは文字数（末尾空白詰め）, nは10485760（1GB）まで|
|VARCHAR(n)|可変長の文字列|nは文字数, nは10485760（1GB）まで|
|DATE|日付と時刻|1000-01-01 00:00:00 ~ 9999-12-31 23:59:59|
|SERIAL|自動連続番号| - |

|属性|意味|
|:--|:--|
|NOT NULL|NULL値を許さない|
|UNIQUE|一意の値を入れる必要がある|
|PRIMARY KEY|表の主キー|
|REFERENCES 表名(列名)|表の外部キー|
|DEFAULT デフォルト値|デフォルト値の指定|

#### 表の変更

##### 列の構成変更

```sql
# 列の追加
ALTER TABLE 表名 ADD 列名 データ型;

# 列の削除
ALTER TABLE 表名 DROP 列名;
```

##### 列の属性変更

```sql
# PRIMARY KEY の追加
ALTER TABLE 表名 ADD PRIMARY KEY (列名);

# PRIMARY KEY の削除
ALTER TABLE 表名 DROP CONSTRAINT 主キー名;


# NOT NULL の追加
ALTER TABLE 表名 ALTER 列名 SET NOT NULL;

# NOT NULL の削除
ALTER TABLE 表名 ALTER 列名 DROP NOT NULL;


# データ型の変更
ALTER TABLE 表名 ALTER 列名 TYPE データ型;
```

#### 表の削除

```sql
DROP TABLE 表名;
```

### シーケンス

#### シーケンスの作成

```sql
CREATE SEQUENCE シーケンス名 
    [INCREMENT 増分値] 
    [MINVALUE 最小値] 
    [MAXVALUE 最大値] 
    [START 初期値]
```

#### シーケンスの関数

```sql
# 次のシーケンス番号を返す
NEXTVAL('シーケンス名')

# 現在のシーケンス番号を返す
CURRVAL('シーケンス名')

# シーケンス番号の現在値に指定した値を設定する
SETVAL('シーケンス名', 値)
```

#### シーケンスの削除

```sql
DROP SEQUENCE シーケンス名;
```

### ビュー

#### ビューの作成

```sql
CREATE VIEW ビュー名 (列名, ...) AS 副問い合わせ文
```

#### ビューの検索

```sql
SELECT 列名, ... FROM ビュー名 WHERE 条件式;
```

#### ビューの変更

##### データの追加

```sql
# すべての列の値を指定
INSERT INTO ビュー名 VALUES(値1, ...);

# 特定の列の値を指定
INSERT INTO ビュー名 (列名1, ...) VALUES(値1, ...);
```

- ビューからのデータ追加の制限

```sql
CREATE VIEW ビュー名 AS 副問い合わせ文 WITH CHECK OPTION;
```

#### ビューの削除

```sql
DROP VIEW ビュー名;
```

#### ビューの定義確認

```sql
SELECT definition FROM pg_views 
    WHERE viewname = 'ビュー名';
```

### インデックス

#### インデックスの作成

```sql
CREATE INDEX インデックス名 ON 表名 (列名, ...);
```

#### インデックスの削除

```sql
DROP INDEX インデックス名;
```

- 大規模ダミーデータの作成方法

```sql
# STEP1: 基になる表の作成
CREATE TABLE base (
    id SERIAL PRIMARY KEY, 
    val INT NOT NULL DEFAULT 0
);

# STEP2: 基になるデータの作成
INSERT INTO base (val) 
    VALUES 
        (1), 
        (2), 
        (3), 
        (4), 
        (5), 
        (6), 
        (7), 
        (8), 
        (9), 
        (10);

# STEP3: ダミーデータ用の表を作成
CREATE TABLE sample (
    id SERIAL PRIMARY KEY,
    name TEXT
);

# STEP4: ダミーデータの作成（100万件）
INSERT INTO sample(name) 
    SELECT 'name' || NEXTVAL('sample_id_seq') 
    FROM 
        base b1, 
        base b2, 
        base b3, 
        base b4, 
        base b5, 
        base b6;
```

### ロール（ユーザ）

#### ロールの作成

```sql
CREATE ROLE|USER ロール名 [[WITH] 属性 [...]];
```

#### ロールの変更

```sql
ALTER ROLE ロール名 [[WITH] 属性 [...]];
```

#### ロールの削除

```sql
DROP ROLE|USER ロール名[, ...];
```

|属性|意味|
|:--|:--|
|LOGIN / NOLOGIN|ログイン権限の有無|
|SUPERUSER / NOSUPERUSER|管理ユーザ権限の有無|
|CREATEDB / NOCREATEDB|データベース作成権限の有無|
|CREATEROLE / NOCREATEROLE|ロール作成権限の有無|
|[ENCRYPTED] PASSWORD 'パスワード'|ユーザのパスワードの設定（ENCRYPTEDを指定すると暗号化される）|

## データ制御言語（DDL）

### COMMIT文

```sql
COMMIT;
```

### ROLLBACK文

```sql
# トランザクション開始からのすべての更新をキャンセル
ROLLBACK;

# トランザクションの途中のセーブポイント以降の更新をキャンセル
ROLLBACK TO セーブポイント名;
```

### SAVEPOINT文

```sql
SAVEPOINT セーブポイント名
```

### GRANT文

#### 特定のデータベースへのアクセス権限の付与

```sql
GRANT 権限 ON DATABASE データベース名 TO ロール名;
```

|権限|意味|
|:--|:--|
|CONNECT|データベースに接続が可能|
|CREATE|データベース内にスキーマを作成可能|

#### 特定のテーブルへのアクセス権限の付与

```sql
GRANT 権限 ON テーブル名 TO ロール名;
```

|権限|意味|
|:--|:--|
|SELECT / INSERT / UPDATE / DELETE / TRUNCATE|SQL文を実行可能 ＆ 列単位で指定可能|
|REFERENCE|外部キーで参照可能|

#### 特定のスキーマへのアクセス権限の付与

```sql
GRANT 権限 ON スキーマ名 TO ロール名
```

|権限|意味|
|:--|:--|
|USAGE|スキーマ内のオブジェクトへのアクセスが可能|
|CREATE|スキーマ内のオブジェクトを作成可能|

### REVOKE文

#### 特定のデータベースへのアクセス権限の削除

```sql
REVOKE 権限 ON DATABASE データベース名 FROM ロール名;
```

#### 特定のテーブルへのアクセス権限の削除

```sql
REVOKE 権限 ON テーブル名 FROM ロール名;
```

#### 特定のスキーマへのアクセス権限の削除

```sql
REVOKE 権限 ON スキーマ名 FROM ロール名;
```