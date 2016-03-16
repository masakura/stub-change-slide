# 草案


## 課題
```csharp
private void ClickButton1(object sender, EventArgs e)
{
    // サービスがまだできていないのでコメントにしておく
    // var request = WebRequest.Create('...');
    // var response = request.GetResponse();
    // var serializer = new DataContractSerializer(typeof(Result));
    // var result = (Result) serializer.ReadObject(response.getResponseStream();

    // 一時的にスタブを使う
    var result = new Result
    {
        Title = "たいとる1",
        Description = "説明1",
        // ... 以降データ作成
    };

    TitleTextBox.Text = result.Title;
    DescriptionTextBox.Text = result.Description;
    // ... 以降データをフォームに設定する

    // using とか...
}
```

こういう感じで、ウェブサービスの実装が終わってないけど画面を作りたい! というときにこんな感じでスタブを埋め込んだりする。

だけど、この方法は問題が多い。

スタブと本番コードの切り替えに失敗することが多い。このような単純なケースでも、本番コード部分のコメントを外す、スタブコードをコメントにという手順を取らないといけない。

* 切り替えによく失敗して、うまく動かなくてパニクる
* 動かないならまだしも、信じられないバグを生んだり...
* スタブを有効にしたままバージョン管理にコミットしてしまう

実際のプロジェクトでは、スタブはもっと複雑で、例えば、入力したコードに合わせたデータを使って、更にそれらを計算したものを表示する場合、コードのあらゆるところにスタブを書く必要があり、切り替えを困難にする。


## 工夫
### スタブと本番コードの実装を分ける
```csharp
public class HogeClient
{
    public virtual Result GetHoge(int id)
    {
        var request = WebRequest.Create('...');
        var response = request.GetResponse();
        var serializer = new DataContractSerializer(typeof(Result));
        return (Result) serializer.ReadObject(response.getResponseStream();
    }

    public static HogeClient Current { get; set; }
}

public class HogeClientStub : HogeClient
{
    public override Result GetHoge(int id)
    {
        return new Result
            {
                Title = "たいとる1",
                Description = "説明1",
                // ... 以降データ作成
            };
    }
}
```

こんな感じにしておいて...

```csharp
private void ClickButton1(object sender, EventArgs e)
{
    // サービスがまだできていないのでコメントにしておく
    // var client = new HogeClient();

    // 一時的にスタブを使う
    var client = new HogeClientStub();

    // 本番コードかスタブかわかんないけど、データを取得する
    var result = client.GetHoge(1);

    TitleTextBox.Text = result.Title;
    DescriptionTextBox.Text = result.Description;
    // ... 以降データをフォームに設定する

    // using とか...
}
```

依然として、コメントの解除とコメントアウトの必要があるけど、一行だけで済むので間違いがかなり減る。


### コードの切り替えすらなくす

```csharp
public class HogeClient
{
    static HogeClient()
    {
        Current = new HogeClient();
    }

    public static HogeClient { get; set; }

    public virtual Result GetHoge(int id)
    {
        var request = WebRequest.Create('...');
        var response = request.GetResponse();
        var serializer = new DataContractSerializer(typeof(Result));
        return (Result) serializer.ReadObject(response.getResponseStream();
    }

    public static HogeClient Current { get; set; }
}
```

```csharp
private void ClickButton1(object sender, EventArgs e)
{
    // 本番コードかスタブかわからんけど、通信オブジェクトをゲット
    var client = HogeClient.Current;

    // 本番コードかスタブかわかんないけど、データを取得する
    var result = client.GetHoge(1);

    TitleTextBox.Text = result.Title;
    DescriptionTextBox.Text = result.Description;
    // ... 以降データをフォームに設定する

    // using とか...
}
```

こうしておいて... 新しく別のプロジェクトを作る。元のプロジェクトは参照しておく。

```csharp
public class HogeClientStub : HogeClient
{
    public override Result GetHoge(int id)
    {
        return new Result
            {
                Title = "たいとる1",
                Description = "説明1",
                // ... 以降データ作成
            };
    }
}
```

こうしておいて...

``csharp
public static void Main()
{
    // スタブが使われるようにする
    HogeCient.Current = new HogeClientStub();

    // 元のプロジェクトを呼び出す
    HogeApp.Program.Main();
}
```

こうすると、実行するプロジェクトの選択で本番コードが使われるかスタブが使われるかを選択できる。

* コードの書き換えは一切起きないため、失敗しない
* 本番コードに一切スタブが混じらない
* コードレビューが楽になる
  - 本番コードに変なコミットが混ざらない
  - スタブのプロジェクトは少々変なコミットをしても問題ない


## 発展して...
### サービスロケーターや DI
基本的に、オブジェクトを作る人と使う人に役割を分けると幸せになれる。書籍「オブジェクト指向のこころ」より。

この仕組みをクラス一つ一つに作ると面倒なので、サービスロケーターや DI を使うといい感じになる。これらを使うと、コンポーネントを組み合わせて利用できるようになる。


### キャッシュを埋め込む
プロキシパターンが使える。


### クラサバ型をスタンドアローンに
今時そんな要求はないと思うけど... クラサバ版とスタンドアローン版を同時に出すとかの時もこれが使える。

サーバー側の実装を別のアプリケーションドメインで動作させておいて (.NET Framework のアプリケーションドメイン) RPC で呼び出す実装を作る。この二つのパターンを同じインターフェイスで呼び出せるようにしておいて、一気に切り替えられるようにするとかなり便利。


### アダプターパターン
アダプターパターンと組み合わせることによって、似たような別のサービスに接続するように書くのも比較的簡単にできる。


## 資料
* [オブジェクト指向のこころ](http://www.amazon.co.jp/dp/4621066048)
