## Extensions
- Python (wich should install Pylance for static type checking)
- Ruff, one tool for linting, black formatting, isort etc
- Jupyter (Keymap, Slide show, cell tags)
- Coverage gutters for code coverage
- Remote ssh/explorer to connect to distant servers easily (no need for jupyther hub now)
- GitLens to supercharge git
- Vim
- Material icon theme
- Theme (publisher:"Mhammed Talhaouy"), personnal preference

## Configuration file
Here is my config file,  useful to handle copy paster in vim, or autolaunch ruff on save for example:
```
{
	"editor.minimap.enabled": false,
	// Bracket-pair colorization
	"editor.bracketPairColorization.enabled": false,
	"notebook.diff.ignoreMetadata": true,
	"gitlens.hovers.currentLine.over": "line",
	"workbench.iconTheme": "material-icon-theme",
	"files.autoSave": "afterDelay",
	"git.confirmSync": false,
	"jupyter.notebookFileRoot": "${workspaceFolder}",
	"vim.commandLineModeKeyBindingsNonRecursive": [],
	"vim.useSystemClipboard": true,
	"vim.handleKeys": {
		"<C-c>": false,
		"<C-v>": false
	},
	"vim.visualModeKeyBindingsNonRecursive": [
	{
		"before": [
			"p",
		],
		"after": [
			"p",
			"g",
			"v",
			"y"
		]
	}
	],
	"[python]": {
		"editor.formatOnSave": true,
		"editor.codeActionsOnSave": {
		"source.organizeImports": true,
		"source.fixAll": true
		},
		"editor.formatOnType": true,
	},
	"python.analysis.typeCheckingMode": "basic",
	"python.analysis.autoImportCompletions": true,
	"python.analysis.stubPath": "",
	"python.analysis.indexing": true,
	"python.terminal.activateEnvironment": false,
	"workbench.colorTheme": "Theme",
	"gitlens.views.branches.branches.layout": "list",
	"explorer.confirmDragAndDrop": false,
	"files.exclude": {
	"**/__pycache__": true,
	"**/.pytest_cache": true
	},
	"python.analysis.inlayHints.pytestParameters": true,
	"pythonTestExplorer.testFramework": "pytest",
	"python.analysis.inlayHints.functionReturnTypes": false,
}
```

## Testing
```
poetry add pytest pytest-cov
```
The first one will give a coverage visible directly in vs code, the other one a report inside the terminal
```
pytest . --cov-report xml:cov.xml --cov .
pytest . --cov-report term --cov .
```

## Profiling
```
poetry add py-spy
```
You can then launch the py-spy to sample the running process and get a nice svg visualization:
```
py-spy record --pid 1400174 --format speedscope -r 1000
```
