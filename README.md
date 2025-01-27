# How to set up a new robot project

Instructions also exist at https://docs.wpilib.org/en/latest/docs/zero-to-robot/step-4/creating-test-drivetrain-program-cpp-java-python.html. If you get stuck, refer to the linked WPILib documentation.

## Making the project

Open up the Command Palette (Ctrl + Shift + P)

Search up `WPILib: Create a new project` and hit it.

Select the template project type, use Java, then select `Command Robot Skeleton (Advanced)`.

Choose a folder to place your project in.

Choose a project name.

Put in your team number (20).

Check the `Enable Desktop Support` checkbox.

Click `Generate Project`.

Open the folder and trust all the authors.

## Modifying the project to fit our needs

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

After that, run the `spotlessApply` Gradle task to format everything. You can do this by opening up the Command Palette and using `WPILib: Run a command in Gradle` and inputting `spotlessApply`.

Go to `settings.json` and paste this section in before the last curly brace (you'll need to put a comma on the line right before you paste this in):
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

Also, add in `"edu.wpi.first.wpilibj2.command.Commands.*"` to the list under `java.completion.favoriteStaticMembers`. This will automatically cause inline command factory methods from the `Commands` class to show up in autocomplete without having to statically import them first. Subsystem inline command factory methods will still be prioritized, so you don't need to worry about selecting the wrong one.

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
	"java.import.gradle.annotationProcessing.enabled": false,
	"java.completion.favoriteStaticMembers": [
		"org.junit.Assert.*",
		"org.junit.Assume.*",
		"org.junit.jupiter.api.Assertions.*",
		"org.junit.jupiter.api.Assumptions.*",
		"org.junit.jupiter.api.DynamicContainer.*",
		"org.junit.jupiter.api.DynamicTest.*",
		"org.mockito.Mockito.*",
		"org.mockito.ArgumentMatchers.*",
		"org.mockito.Answers.*",
		"edu.wpi.first.units.Units.*",
		"edu.wpi.first.wpilibj2.command.Commands.*"
	],
	"java.completion.filteredTypes": [
		"java.awt.*",
		"com.sun.*",
		"sun.*",
		"jdk.*",
		"org.graalvm.*",
		"io.micrometer.shaded.*",
		"java.beans.*",
		"java.util.Base64.*",
		"java.util.Timer",
		"java.sql.*",
		"javax.swing.*",
		"javax.management.*",
		"javax.smartcardio.*",
		"edu.wpi.first.math.proto.*",
		"edu.wpi.first.math.**.proto.*",
		"edu.wpi.first.math.**.struct.*",
	],
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

Copy the `build.yml` file in this repo to the `.github/workflows` directory. The path to the file should be `.github/workflows/build.yml`. This enables CI checks to be run on the repository, which is important to ensure that the code builds and is formatted properly. Once you publish the robot code repo, make sure you go to the Actions tab and enable workflows so they actually run.

The repo should now be ready for others to start using.

## Making the git repo and publishing to GitHub

There are multiple ways of doing this. The easiest way to do this is with the git CLI. Pull up a terminal and ensure you're in the robot project's folder. The easiest way to do this is to hit Ctrl + Shift + \`. Then, type in `git init -b main`. This will create the git repo with a branch called `main`. You can now use GitHub Desktop or VS Code to commit all the files.

You can now publish the repo to GitHub. There are multiple ways of doing this. One way is to use the web interface by clicking the green `New` button on the team on Team 20's GitHub page which will lead you [here](https://github.com/organizations/team20/repositories/new); then you can give the repo a name, make sure the repo is public so we don't have to pay for CI, and click Create repository. You should be able to follow the instructions for pushing an existing repo from the command line. Those instructions will add the GitHub repo as the remote repo (somewhere you can push commits to) and then push your changes to the GitHub repo. Just make sure the last part of the `git push` command is your branch name (should be `main`). `git push -u origin main` will push a branch called `main` to the remote repo called `origin` (the GitHub repo). `-u` will link the branch on GitHub to your local branch so changes can be synchronized to your local repo and back to GitHub.

## Configuring the GitHub repo

Make sure you go to the Actions tab and enable workflows so they actually run and our code gets checked.

Turn on branch protection to prevent pushing to the main branch. You can do this by going into the Settings tab of the robot code repo, clicking on Branches, clicking the `Add rule` button, typing in `main` for the Branch name pattern, and checking the following checkboxes: Require a pull request before merging, Require status checks to pass before merging, and Do not allow bypassing the above settings. This ensures no one can push to main, not even admins.
