# Tools

On the **Tools** preferences page you can define external tools which can operate on certain selections.

The configuration is stored in `tools.yml` file in the [settings directory](Installation-and-Files.md), but it might be easier to use the **Export** and **Import** buttons to share your configuration with team members.

## Default Tools

By default, some default tools will be configured on the first application start.

The **Open File** tool will invoke the system's default open command, e.g. to launch a graphic viewer for `.png` files.
The **Open in Terminal** tool will open the selected directory in the terminal application.
The **Open Git-Shell** tool will open the repository in the Git shell.

To re-add default tools, click the **Re-Add Defaults** button.

### Note

> With SmartGit 22.1 the default tools have been cleaned up.
> The Git version is now displayed in the preferences, *Fast-forward Merge* has been implemented as SmartGit command.
>
> If you are missing further tools from SmartGit version before 22.1 (e.g. Set Executable Flag, Format Patch, Apply Patch, Verify Database or Git Config), please download this file for [Windows](pre-22.1-tools-win.yml) or [Linux/MacOS](pre-22.1-tools-nix.yml) and *Import* that in SmartGit.

## Configure a Tool

Use **Add** or **Copy** to create a new tool (using the latter will copy all values from the currently selected tool for easier application of tiny changes).

The **Command** means just the executable file without any argument.
The **Arguments** can contain following variables accessible using the drop-down button at the right:

- `${filePath}` = the path of the selected file or directory, e.g. `C:\repository\readme.txt`
- `${fileName}` = the name of the selected file or directory, e.g. `readme.txt`
- `${fileUri}` = the URI of the selected file or directory, e.g. `file://C:/repository/readme.txt`
- `${repositoryRootPath}` = the root path of the repository, e.g. `C:\repository`
- `${selectionFile}` = the path of a temporary file containing all selected file paths, one file per line, e.g. `C:\temp\selected-91235`
- `${remoteUrl}` = the remote (push) URL of the repository, e.g. `https://github.com/user/project.git`
- `${encoding}` = the configured text file encoding, e.g. `UTF-8`
- `${commit}` = the (single or first) selected commit or ref (depending on the **Handles: Refs, Commits, Both** option), e.g. `feature/new-layout`
- `${commit2}` = the second selected commit or ref, e.g. `feature/old-layout`
- `${fileOpen}` = the path of a existing file that the user will have to select when invoking the tool, e.g. `D:\download\my-patch.txt`
- `${fileSave}` = the path of a file that the user will have to select when invoking the tool; if existing, the user needs to confirm the overwrite
- `${dirSelect}` = the path of a directory that the user will have to select when invoking the tool, e.g. `D:\export`
- `${git}` = the path of the configured Git executable, e.g. `C:\Program Files\SmartGit\git\bin\git.exe`
- `${gitDir}` = the (root) path configured Git installation, e.g. `C:\Program Files\SmartGit\git`
- `${smartGitDir}` = the (root) path of the SmartGit installation, e.g. `C:\Program Files\SmartGit`

The *working directory* when launching the tool will be the root directory of the corresponding repository (which may also be a submodule).
When launching a tool on a set of files which belong to different repositories, it will be the closest common directory of all affected repositories.

If **Can be used by the Open command** is selected, SmartGit will consider to use this tool when invoking **Open** (or **Open from Working Tree**) in the **Files** view.
The **Handles: Files, Directories, Both** and **Handles: Refs, Commits, Both** options determines on what selection the tool should operate, e.g. on file, directory, ref (tags or branches) or commit selection.

A file or directory name pattern may be specified in **Applies To**.
#### Note
> For repository root directories, the name "" (empty string) is used as name which only is matched by the pattern "*".


If **Request confirmation before invoking** is selected and a message is provided, the user needs to confirm this dialog's message before the command is invoked.
If **Show output and wait until finished** is selected, SmartGit waits until the command is finished and shows the output.

## Example Tools

### Open top-most Visual Studio solution from current repository

This powershell script can be used to open a solution file `.sln` inside a repository, in Visual Studio.
The script iterates through the repository folders and looks for a `.sln` file.
It stops at the first match and then launches Visual Studio.

![](attachments/53215435/53215436.png)

**Powershell script C:\\SmartGit-Scripts\\openVS.ps1**

``` java
Install-Module VSSetup -Scope CurrentUser
$slnname = Get-ChildItem -Path $args[0] -Filter *.sln -Recurse -ErrorAction SilentlyContinue -Force | Select-Object -First 1 | Select-Object -ExpandProperty FullName

switch ( $args[1] )
{
    2017
    {
        $ver = '[15.0,16.0)'
    }

    2019
    {
        $ver = '[16.0,17.0)'
    }

    default
    {
        $ver = '[1.0,1000.0)'
    }
}

$devenv = Get-VSSetupInstance | Select-VSSetupInstance -Version $ver -Latest | Select-Object -ExpandProperty InstallationPath
$devenv = $devenv + "\Common7\IDE\devenv.exe"
Start-Process -FilePath $devenv -ArgumentList $slnname
```
