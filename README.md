# How to set up a new robot project

Instructions also exist at https://docs.wpilib.org/en/latest/docs/zero-to-robot/step-4/creating-test-drivetrain-program-cpp-java-python.html. If you get stuck, refer to the linked WPILib documentation.

Open up the Command Palette (Ctrl + Shift + P)

Search up `WPILib: Create a new project` and hit it.

Select the template project type, use Java, then select `Command Robot Skeleton (Advanced)`.

Choose a folder to place your project in.

Choose a project name.

Put in your team number (20).

Check the `Enable Desktop Support` checkbox.

Click `Generate Project`.

Open the folder and trust all the authors.

Copy the `eclipse-formatter.xml` file in the repo into your robot project.

Next you'll setup code formatting. Instructions for setup exist at https://docs.wpilib.org/en/latest/docs/software/advanced-gradlerio/code-formatting.html, but this will also have instructions for convenience.

Go to `build.gradle` and copy and add spotless as a plugin by adding `id 'com.diffplug.spotless' version '6.20.0'` to the `plugins` block. The version will change over time, you can look at https://github.com/diffplug/spotless/blob/main/plugin-gradle/CHANGES.md for the latest version. The start of your `build.gradle` file should look like this (versions will be different):
```groovy
plugins {
   id "java"
   id "edu.wpi.first.GradleRIO" version "2025.1.1"
   id 'com.diffplug.spotless' version '6.20.0'
}
```

Paste in this spotless block after the `plugins` block to configure spotless to format your code:
```gradle
spotless {
    java {
        target fileTree('.') {
            include '**/*.java'
            exclude '**/build/**', '**/build-*/**'
        }
        removeUnusedImports()
        eclipse().configFile("eclipse-formatter.xml")
    }
    groovyGradle {
        target fileTree('.') {
            include '**/*.gradle'
            exclude '**/build/**', '**/build-*/**'
        }
        greclipse()
        indentWithSpaces(4)
        trimTrailingWhitespace()
        endWithNewline()
    }
}
```

Go to `settings.json` and paste this section in before the last curly brace(you'll need to put a comma on the line right before you paste this in):
```json
	"C_Cpp.clang_format_fallbackStyle": "{BasedOnStyle: Google, ColumnLimit: 0, IndentWidth: 4, TabWidth: 4, UseTab: ForIndentation}",
	"editor.codeActionsOnSave": {
		"source.organizeImports": "explicit"
	},
	"editor.formatOnSave": true,
	"editor.detectIndentation": false,
	"editor.insertSpaces": false,
	"java.format.settings.url": "eclipse-formatter.xml",
	"java.sources.organizeImports.staticStarThreshold": 1
```

Your `settings.json` should look similar to this (it's fine if there's a few differences in the part that already existed, since that's WPILib's part):
```json
{
	"java.configuration.updateBuildConfiguration": "automatic",
	"java.server.launchMode": "Standard",
	"files.exclude": {
		"**/.git": true,
		"**/.svn": true,
		"**/.hg": true,
		"**/CVS": true,
		"**/.DS_Store": true,
		"bin/": true,
		"**/.classpath": true,
		"**/.project": true,
		"**/.settings": true,
		"**/.factorypath": true,
		"**/*~": true
	},
	"java.test.config": [
		{
			"name": "WPIlibUnitTests",
			"workingDirectory": "${workspaceFolder}/build/jni/release",
			"vmargs": [
				"-Djava.library.path=${workspaceFolder}/build/jni/release"
			],
			"env": {
				"LD_LIBRARY_PATH": "${workspaceFolder}/build/jni/release",
				"DYLD_LIBRARY_PATH": "${workspaceFolder}/build/jni/release"
			}
		},
	],
	"java.test.defaultConfig": "WPIlibUnitTests",
	"C_Cpp.clang_format_fallbackStyle": "{BasedOnStyle: Google, ColumnLimit: 0, IndentWidth: 4, TabWidth: 4, UseTab: ForIndentation}",
	"editor.codeActionsOnSave": {
		"source.organizeImports": "explicit"
	},
	"editor.formatOnSave": true,
	"editor.detectIndentation": false,
	"editor.insertSpaces": false,
	"java.format.settings.url": "eclipse-formatter.xml",
	"java.sources.organizeImports.staticStarThreshold": 1
}
```
