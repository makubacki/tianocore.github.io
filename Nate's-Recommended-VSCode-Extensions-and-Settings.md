Visual Studio Code is currently my preferred text editor for working on EDK II code. However, making it a good environment for EDK II development work requires several extensions to be installed. My recommendations are below:

# Recommended Extensions

* [C/C++ Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
* [x86 and x86_64 Assembly Syntax Highlighting](https://marketplace.visualstudio.com/items?itemName=13xforever.language-x86-64-assembly)
* [Edk2code](https://marketplace.visualstudio.com/items?itemName=intel-corporation.edk2code)
    * Provides syntax highlighting for ASL, DEC, DSC, FDF, INF, UNI, VFR
    * On Linux one must install cscope for this extension to work `sudo apt install cscope` or similar.
* [Python Tools](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
* [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
* [add-contextmenu-toggle-comment](https://marketplace.visualstudio.com/items?itemName=ebicochineal.add-contextmenu-toggle-comment)
* [Bookmarks](https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks)
* [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
* [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory)
* [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)

# Recommended Settings

I recommend you place these settings into your VSCode settings.json file:

```
{
    "files.associations": {
        "*.fdf": "edk2_fdf",
        "*.dsc": "edk2_dsc",
        "*.dec": "edk2_dec",
        "*.inf": "edk2_inf",
        "*.aslc": "c",
        "*.nasmb": "asm",
        "*.asm16": "asm"
    },
    "files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/CVS": true,
        "**/.DS_Store": true,
        "**/Autogen.c": true,
        "Build/**": true,
    },
    "editor.renderWhitespace": "all",
    "files.trimTrailingWhitespace": true,
    "editor.tabSize": 2,
    "editor.rulers": [
        80,
        120
    ],
}
```

### Obsolete Extensions
This functionality is now provided by Edk2code
* ~~[ASL Syntax Highlighting](https://marketplace.visualstudio.com/items?itemName=Thog.vscode-asl)~~
* ~~[DEC/DSC/FDF/INF/UNI/VFR Syntax Highlighting](https://marketplace.visualstudio.com/items?itemName=walonli.edk2-vscode)~~