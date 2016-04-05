#### スタブも委譲する

```csharp
public partial class GitHubForm : Form
{
    public GitHubForm()
    {
        // ここで切り替える
        GitHubClient = new GitHubClient();
        // GitHubClient = new GitHubClientStub();

        // ...
    }

    private GitHubClient GitHubClient { get; }

    private async void loadButton_Click(object sender, EventArgs e)
    {
        var repositories = await GitHubClient.GetGitHubRepositories();

        repositoriesDataGrid.DataSource = repositories;
    }
}
```
