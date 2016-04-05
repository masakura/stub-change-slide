#### 切り替えを GitHubClient に移動する

```csharp
public class GitHubClient
{
    // ここで切り替える
    public static GitHubClient Current { get; } = new GitHubClient();
    // public static GitHubClient Current { get; } = new GitHubClientStub();

    // ...
}
```

```csharp
public partial class GitHubForm : Form
{
    public GitHubForm()
    {
        GitHubClient = GitHubClient.Current;

        // ...
    }

    // ...
}
```
