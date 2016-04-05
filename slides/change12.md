#### 委譲クラスの作成

新しいクラスを作成し、先ほど抽出したメソッドを移動

```csharp
public class GitHubClient
{
    public virtual async Task<object> GetGitHubRepositories()
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("User-Agent", "My App");
            var response = await client.GetAsync("https://api.github.com/repositories");

            var text = await response.Content.ReadAsStringAsync();
            return JsonConvert.DeserializeObject(text);
        }
    }
}
```
