#### 委譲する

```csharp
public partial class GitHubForm : Form
{
    public GitHubForm()
    {
        // 委譲クラスをインスタンス化
        GitHubClient = new GitHubClient();

        // ...
    }

    private GitHubClient GitHubClient { get; }

    private async void loadButton_Click(object sender, EventArgs e)
    {
        var repositories = await GitHubClient.GetGitHubRepositories();
        // var repositories = await GetGitHubRepositoriesStub();

        repositoriesDataGrid.DataSource = repositories;
    }
}
```
