### 一番簡単な方法

```csharp
private async void loadButton_Click(object sender, EventArgs e)
{
    var repositories = await GetGitHubRepositories();
    // var repositories = await GetGitHubRepositoriesStub();

    repositoriesDataGrid.DataSource = repositories;
}

private static async Task<object> GetGitHubRepositories()
{
    // ここは本番用コード...

    return repositories;
}

private static async Task<object> GetGitHubRepositoriesStub()
{
    // ここはスタブコード...

    return repositories;
}
```
