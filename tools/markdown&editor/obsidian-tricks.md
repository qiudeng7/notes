# Obsidian tricks
## Terminal Plugin
set `ctrl + ~` to open powershell terminal as the same with vscode.
plugin settings as following:
```json
{
	"addToCommand": true,
	"addToContextMenu": false,
	"createInstanceNearExistingOnes": true,
	"errorNoticeTimeout": 0,
	"exposeInternalModules": true,
	"focusOnNewInstance": true,
	"hideStatusBar": "never",
	"interceptLogging": true,
	"language": "",
	"newInstanceBehavior": "newTab",
	"noticeTimeout": 5,
	"openChangelogOnUpdate": false,
	"pinNewInstance": false,
	"preferredRenderer": "webgl",
	"profiles": {
		"7ddb5223-6900-4ee3-ac90-c96cae7e274a": {
			"args": [],
			"executable": "powershell",
			"name": "",
			"platforms": {
				"win32": true
			},
			"restoreHistory": true,
			"successExitCodes": [
				"0",
				"SIGINT",
				"SIGTERM"
			],
			"terminalOptions": {
				"documentOverride": null
			},
			"type": "external"
		}
	}
}
```
## Other Plugins
```toml

word splitting for simplified Chinese in Edit Mode and Vim Mode

# custom css
style settings
css snippts

paste URL into selection
```

