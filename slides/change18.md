#### アプリのブートストラップを抽出

`Main` クラスのブートストラップコードを外部から呼び出せるようにする

```csharp
public static class Bootstrap
{
    public static void Run()
    {
        // もともと、Program.Main にあったコード
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new GitHubForm());
    }
}
```

```csharp
internal static class Program
{
    [STAThread]
    private static void Main()
    {
        Bootstrap.Run();
    }
}
```
