#### スタブプロジェクトのブートストラップコードを書く

```csharp
namespace Stub
{
    internal static class Program
    {
        [STAThread]
        private static void Main()
        {
            // スタブを使うようにする
            GitHubClient.Current = new GitHubClientStub();

            Bootstrap.Run();
        }
    }
}
```
