# クラウド同期の設定手順（Supabase）

スマホで操作してタブレットに表示する、のように **別々の端末・別々の回線** で使うための設定です。
最初に 1 回だけ設定すれば、あとは GitHub Pages の URL を開くだけで使えます。所要時間は 15 分ほどです。

> この設定をしなくても、同じ PC の同じブラウザ内での同期はそのまま使えます。

---

## 1. Supabase のプロジェクトを作る

1. [https://supabase.com](https://supabase.com) にアクセスし、「Start your project」から GitHub アカウントなどでサインインします。
2. 「New project」を押し、次のように入力します。
   - **Name**：任意（例：`ems-monitor`）
   - **Database Password**：任意（このアプリでは使いません。忘れないように控えておけば十分です）
   - **Region**：`Northeast Asia (Tokyo)` を選ぶと遅延が少なくなります。
   - **Plan**：Free（無料）
3. 「Create new project」を押し、作成が終わるまで 1〜2 分待ちます。

## 2. URL とキーをコピーする

プロジェクトの画面で「Connect」ボタン、または **Project Settings → API Keys** を開き、次の 2 つをコピーします。

| 項目 | 例 |
|---|---|
| Project URL | `https://abcdefghijklmnop.supabase.co` |
| Publishable key（または anon key） | `sb_publishable_...` または `eyJhbGciOi...` |

> [!CAUTION]
> **`secret` / `service_role` のキーは絶対に使わないでください。** これらは管理者用のキーで、公開すると危険です。
> Publishable key（anon key）は、ブラウザに置いて公開することを前提にしたキーなので、GitHub に載せても問題ありません。

## 3. Realtime の設定を確認する

**Realtime → Settings** を開き、「**Allow public access**」が **ON（有効）** になっていることを確認します。初期設定は ON です。

> テーブルの作成やデータベースの設定は不要です。このアプリはデータを保存せず、Realtime の Broadcast（一時的なメッセージ中継）だけを使います。

## 4. index.html に貼り付ける

`index.html` をテキストエディタ（メモ帳、VS Code など）で開き、先頭付近にある次の部分を探します。

```js
const CLOUD = {
  url: '',   // 例: 'https://abcdefghijklmnop.supabase.co'
  key: '',   // 例: 'sb_publishable_xxxxxxxx' または 'eyJhbGciOi...'（anon key）
};
```

`''` の中に、コピーした値を貼り付けます。

```js
const CLOUD = {
  url: 'https://abcdefghijklmnop.supabase.co',
  key: 'sb_publishable_xxxxxxxxxxxxxxxx',
};
```

保存したら、GitHub の `index.html` を差し替えます。

## 5. 使う

1. **タブレット**（モニター）で `https://ユーザー名.github.io/リポジトリ名/?mode=monitor` を開きます。
   画面に 4 桁の **ルーム番号** が表示されます（例：`8025`）。
2. **スマホ**（操作）で `https://ユーザー名.github.io/リポジトリ名/?mode=control` を開きます。
   「クラウド同期」欄にルーム番号を入力して「接続」を押します。
3. 「✓ ROOM 8025 のモニターと接続しています」と表示されたら準備完了です。数値を入れて「反映」を押してください。

- ルーム番号は端末に保存されるので、次回からは自動で同じ番号につながります。
- ルーム番号を固定したいときは、モニターの URL の末尾に `&room=1234` を付けます（例：`?mode=monitor&room=1234`）。
- スマホとタブレットは、別々の回線（4G/5G と Wi-Fi など）でも、離れた場所にあってもつながります。

---

## 困ったとき

| 症状 | 確認すること |
|---|---|
| 「クラウド接続エラー」と表示される | URL とキーの貼り付け間違い（前後の空白、`'` の消し忘れ）がないか。Realtime の「Allow public access」が ON か。 |
| 「ネット未接続」と表示される | 端末がインターネットにつながっているか。職場の Wi-Fi で制限されている場合は、スマホの回線に切り替える。 |
| 「モニターの応答待ち」のまま | 操作画面とモニターのルーム番号が一致しているか。モニター画面が開いたままになっているか。 |
| しばらく使わなかったら接続できない | Supabase の無料プランは、一定期間使わないとプロジェクトが一時停止されることがあります。Supabase のダッシュボードを開き、「Restore project」で再開してください。 |

## 注意

- 送受信するのは模擬の数値・リズムの設定だけで、データベースには何も保存されません。
- ルーム番号とキーを知っている人は、同じルームに接続できます。個人情報は入力しないでください。
- 無料プランには通信量の上限があります。訓練で使う程度なら、通常は上限に届きません。
