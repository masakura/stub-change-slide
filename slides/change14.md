#### スタブも同様に委譲クラスを作成

```csharp
public class GitHubClientStub : GitHubClient
{
    public override async Task<object> GetGitHubRepositories()
    {
        return new[]
        {
            new {name = "repo1", full_name = "masakura/repo1", description = "description repo1"},
            new {name = "repo2", full_name = "a/longlongreponame", description = "description repo2"},
            new {name = "repo3", full_name = "masakura/repo3", description = "description repo3"},
            new {name = "repo4", full_name = "masakura/repo4", description = "long long description..."}
        };
    }
}
```
