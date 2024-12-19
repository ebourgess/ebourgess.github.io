---
layout: post
title:  "[Windows Server] Installing NeoVim on Windows Server"
date:   2024-12-19
slug: "installing-neovim-on-windows-server"
authors: ["Elias Bourgess"]
categories: ["Windows Server", "NeoVim"]
draft: false
---

Usually on Windows when you want to install something you simply run 

```powershell
winget install Neovim.Neovim
```

But since this is a `Windows Server` another path is going to be taken. 

## Downloading NeoVim on the Server

In order to download `NeoVim`, simply go to [NeoVim's Github Release Page](https://github.com/neovim/neovim/releases) and download the `.zip` file from there.

## Installing NeoVim

In order to install `NeoVim`, you will need to unzip your archive, and move it to `C:\tools\nvim`. 

After that, you open the `Windows Powershell` **AS ADMIN** and paste the following below:

```powershell
$oldPath = [Environment]::GetEnvironmentVariable("Path", [EnvironmentVariableTarget]::Machine)

[Environment]::SetEnvironmentVariable("Path", "$oldPath;C:\tools\nvim\bin", [EnvironmentVariableTarget]::Machine)

```


## Running NeoVim 

After adding the commands above, quit the shell window and then reopen it and run the following command.

```powershell
nvim 
```

And it should open now.
