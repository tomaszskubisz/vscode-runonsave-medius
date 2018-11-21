# Run On Save for Visual Studio Code (Medius fork)
This extension allows configuring commands that get run whenever a file is saved in vscode.
NOTE: Commands only get run when saving an existing file. Creating new files, and Save as... don't trigger the commands.

Forked from [emeraldwalk.RunOnSave](https://marketplace.visualstudio.com/items?itemName=emeraldwalk.RunOnSave)

## Prerequisites (Medius)

TypeScript compiler:
```sh
$ npm install -g typescript
```

## Features
* Configure multiple commands that run when a file is saved
* Regex pattern matching for files that trigger commands running
* Sync and async support
* Templates for commands group (Medius)

## Configuration
Add "medius.runonsave" configuration to user or workspace settings.
* "shell" - (optional) shell path to be used with child_process.exec options that runs commands.
* "autoClearConsole" - (optional) clear VSCode output console every time commands run. Defaults to false.
* "commands" - array of commands that will be run whenever a file is saved.
  * "match" - a regex for matching which files to run commands on
  > NOTE Since this is a Regex, and also in a JSON string backslashes have to be double escaped such as when targetting folders. e.g. "match": "some\\\\\\\\folder\\\\\\\\.*"
  * "cmd" - command to run. Can include parameters that will be replaced at runtime (see Placeholder Tokens section below).
  * "isAsync" (optional) - defaults to false. If true, next command will be run before this one finishes.
  * "useGroupTemplate" - If true, commands from group template will be created
  * "templateParameters" - Used with useGroupTemplate. Parameters separated by semicolon. Eg. 'Enterprise;c:\repos' will replace placeholders in group templates {$p0}, {$p1}"
* "commandsGroupTemplate" - array of commands templates with placeholders that can be used by commands. Parameter placeholders: ${p0}, ${p1}, etc.

### Sample Config
This sample configuration will run echo statements including the saved file path.
In this sample, the first command is async, so the second command will get executed immediately even if first hasn't completed.
Since the second isn't async, the third command won't execute until the second is complete.

    "medius.runonsave": {
        "commandsGroupTemplate": [
            {
                "match": "\\.ts$",
                "cmd": "tsc --build C:\\Repos\\Project\\${p0}\\tsconfig.json"
            },
            {
                "match": "\\.ts$",
                "cmd": "gulp --cwd C:\\Repos\\Project\\${p0} js"
            },
            {
                "match": "\\.html$",
                "cmd": "gulp --cwd C:\\Repos\\Project\\${p0} html"
            }            
        ]
		"commands": [
			{
				"match": ".*",
				"isAsync": true,
				"cmd": "echo 'I run for all files.'"
			},
			{
				"match": "\\.txt$",
				"cmd": "echo 'I am a .txt file ${file}.'"
			},
			{
				"match": "\\.js$",
				"cmd": "echo 'I am a .js file ${file}.'"
			},
			{
				"match": ".*",
				"cmd": "echo 'I am ${env.USERNAME}.'"
			},
			{
			    "useGroupTemplate": true,
			    "templateParameters": "ComponentABC"
			},
			{
			    "useGroupTemplate": true,
			    "templateParameters": "ComponentDEF"
			}			
		]
	}

## Commands
The following commands are exposed in the command palette:
* On Save: Enable
* On Save: Disable

## Placeholder Tokens
Commands support placeholders similar to tasks.json.

* ${workspaceRoot}: workspace root folder
* ${file}: path of saved file
* ${fileBasename}: saved file's basename
* ${fileDirname}: directory name of saved file
* ${fileExtname}: extension (including .) of saved file
* ${fileBasenameNoExt}: saved file's basename without extension
* ${cwd}: current working directory

### Environment Variable Tokens

* ${env.Name}

## Links
* [Marketplace](https://marketplace.visualstudio.com/items/emeraldwalk.RunOnSave)
* [Source Code](https://github.com/emeraldwalk/vscode-runonsave)

## License
[Apache](https://github.com/emeraldwalk/vscode-runonsave/blob/master/LICENSE)