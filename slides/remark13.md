#### 入り乱れている

```csharp
using (var client = new HttpClient())
{
    // client.DefaultRequestHeaders.Add("User-Agent", "My App");
    // var response = await client.GetAsync("https://api.github.com/repositories");

    // var text = await response.Content.ReadAsStringAsync();
    // var repositories = JsonConvert.DeserializeObject(text);

    var repositories = new[]
    {
        new {name = "repo1", full_name = "masakura/repo1", description = "description repo1"},
        new {name = "repo2", full_name = "a/longlongreponame", description = "description repo2"},
        new {name = "repo3", full_name = "masakura/repo3", description = "description repo3"},
        new {name = "repo4", full_name = "masakura/repo4", description = "long long description..."}
    };

    repositories = from repo in repositories // この行はスタブ? 本番コード?
        select new {name = $"[{repo.name}]", full_name, description};

    repositoriesDataGrid.DataSource = repositories;
}
```

* このコードはちょっと変です... 参考程度としてください
