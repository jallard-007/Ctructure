# Ctructure README

Ctructure is an opinionated, [prettier](https://github.com/prettier/prettier)-inspired code formatter for the C programming language.

- [Commands](#commands)
  - [formatCurrentDocument](#formatcurrentdocument)
  - [formatWorkspaceFolder](#formatworkspacefolder)
  - [formatAllWorkspaceFolders](#formatallworkspacefolders)
  - [createConfigFile](#createconfigfile)
  - [createConfigFileInEveryWorkspaceFolder](#createconfigfileineveryworkspacefolder)
- [Directives](#directives)
  - [no-format](#no-format)
- [Configuration](#configuration)
  - [formatAllWorkspaceFolders.showLogs](#formatallworkspacefoldersshowlogs)
  - [formatWorkspaceFolder.showLogs](#formatworkspacefoldershowlogs)
  - [printer.indentationSize](#printerindentationsize)
  - [printer.indentationType](#printerindentationtype)
  - [printer.lineEndings](#printerlineendings)
  - [printer.lineWidth](#printerlinewidth)
  - [printer.multiVariableAlwaysNewline](#printermultivariablealwaysnewline)
  - [printer.multiVariableMatchIndent](#printermultivariablematchindent)
- [Limitations](#limitations)
- [Issues](#issues)

# Commands

## formatCurrentDocument
Formats the currently active document. To use, [open the command palette](https://www.alphr.com/open-command-vs-code/) and run `Ctructure.formatCurrentDocument`.

## formatWorkspaceFolder
Formats all matching files in a workspace folder. If there are multiple workspace folders, a selection must be made. To use, [open the command palette](https://www.alphr.com/open-command-vs-code/) and run `Ctructure.formatWorkspaceFolder`.

## formatAllWorkspaceFolders
Formats all matching files in all workspace folders. To use, [open the command palette](https://www.alphr.com/open-command-vs-code/) and run `Ctructure.formatAllWorkspaceFolders`.

## createConfigFile
Creates a configuration file (`ctructureconf.json`) in the root of a workspace folder. If there are multiple workspace folders, a selection must be made. To use, [open the command palette](https://www.alphr.com/open-command-vs-code/) and run `Ctructure.createConfigFile`.

## createConfigFileInEveryWorkspaceFolder
Creates a configuration file (`ctructureconf.json`) in every workspace folder. To use, [open the command palette](https://www.alphr.com/open-command-vs-code/) and run `Ctructure.createConfigFileInEveryWorkspaceFolder`.

## convertWorkspaceFolderLineEndings
Converts line endings for all files in a workspace. If there are multiple workspace folders, a selection must be made. To use, [open the command palette](https://www.alphr.com/open-command-vs-code/) and run `Ctructure.convertWorkspaceFolderLineEndings`.

# Directives
Directives modify formatting behavior. They are declared inside comments, beginning with `@ct-`, and are case insensitive.

## no-format
The `@ct-no-format` directive marks a line or section of code to be ignored.
Single-line comment directives (e.g. `// @ct-no-format`) ignore the following line,
2 multi-line comment directives (e.g. `/* @ct-no-format */`) create a section of code to be ignored.

```c
int func_with_ignored_code() {
  // @ct-no-format, ignore next line only
  int a=1, b=2;

  /* @ct-no-format, ignore multiple lines */
  a = (a + b) |
      (a - b) &
      (a * b) ^
      (a / b);
  /* @ct-no-format, end ignored section */

  return a;
}
```

# Configuration

A configuration file can be used to control extension behavior. They must exist in the root of a workspace folder (`workspaceFolder/ctructureconf.json`) and only apply to that single folder. If a workspace has multiple folders, each folder can/should have its own configuration file. If no configuration file exists for a workspace folder, the default configuration (shown below) is used. The [Ctructure.createConfigFile](#createconfigfile) and [Ctructure.createConfigFileInEveryWorkspaceFolder](#createconfigfileineveryworkspacefolder) commands can be used to generate configuration files.

```ts
interface IConfig {
  'formatAllWorkspaceFolders.showLogs': boolean;
  'formatWorkspaceFolder.showLogs': boolean;
  'printer.indentationSize': 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10;
  'printer.indentationType': 'tabs' | 'spaces';
  'printer.lineEndings': 'lf' | 'crlf';
  'printer.lineWidth': number; // must be >0
  'printer.multiVariableAlwaysNewline': boolean;
  'printer.multiVariableMatchIndent': boolean;
}
```

```ts
const defaultConfig: IConfig = {
  'formatAllWorkspaceFolders.showLogs': true,
  'formatWorkspaceFolder.showLogs': true,
  'printer.indentationSize': 2,
  'printer.indentationType': 'spaces',
  'printer.lineEndings': 'lf',
  'printer.lineWidth': 80,
  'printer.multiVariableAlwaysNewline': false,
  'printer.multiVariableMatchIndent': false,
};
```

## formatAllWorkspaceFolders.showLogs
Controls whether a log file will be generated and shown in the editor after running [Ctructure.formatAllWorkspaceFolders](#formatallworkspacefolders).

## formatWorkspaceFolder.showLogs
Controls whether a log file will be generated and shown in the editor after running [Ctructure.formatWorkspaceFolder](#formatworkspacefolder).

## printer.indentationSize
Controls the amount of spaces inserted per indentation level when [printer.indentationType](#printerindentationtype) is set to `spaces`. Has no effect if [printer.indentationType]() is set to `tabs`.

## printer.indentationType
Controls the type of character used for indentation used by the printer. `tabs` will insert 1 tab (`\t`) per indentation level, `spaces` will insert [printer.indentationSize](#printerindentationsize) real spaces per indentation level.

## printer.lineEndings
Controls the end-of-line character sequence used by the printer: `lf` will insert a linefeed (`\n`), `crlf` will insert a carriage-return + linefeed (`\r\n`).

## printer.lineWidth
Controls the maximum ideal number of columns per line. This does not guarantee that all line lengths will fall within this maximum. This setting currently has a margin of error of approx. ±5 characters, so a value of 80 may result in some lines having a width of up to 85 characters (or more if it is not possible to break the line, such as when there are long [string literals](https://docs.microsoft.com/en-us/cpp/c-language/c-string-literals?view=msvc-170) present).

## printer.multiVariableAlwaysNewline
Controls whether variables in a multi-variable declaration/definition will be put on separate lines.

```c
// printer.multiVariableAlwaysNewline OFF (false)
int var1 = 1, var2 = 2, var3;

// printer.multiVariableAlwaysNewline ON (true)
int var1 = 1,
  var2 = 2,
  var3;
```

## printer.multiVariableMatchIndent
Controls whether indentation is matched for all line-separated variables in a multi-variable declaration/definition.

```c
// printer.multiVariableMatchIndent OFF (false)
int var1 = 1,
  var2 = 2,
  var3;

// printer.multiVariableMatchIndent ON (true)
int var1 = 1,
    var2 = 2,
    var3;

// has no effect on one-liners
// (i.e. when printer.multiVariableAlwaysNewline is false)
int x = 1, y = 2, z;
```

# Limitations
- Max supported input file size is 512 MB

# Issues
If you encounter unexpected behavior/failures or just have a suggestion, please create an [issue](https://github.com/nluka/Ctructure/issues). If the issue is a bug, include a copy of the exact file you were trying to format + the error or description of unexpected behavior.
