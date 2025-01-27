# Updating the Readme

Some of the items in the Readme need to updated from time to time. This is comprehensive list of things that need to be updated.

- GradleRIO version. This controls the WPILib version, so at the bare minimum, update the version to the kickoff version every year.
- Spotless versions. Find the latest stable release on https://github.com/diffplug/spotless/blob/main/plugin-gradle/CHANGES.md and update the versions (2 places to update.) Also, ensure that if spotless changed their API, that the spotless block is also updated to keep working.
- `java.completion.favoriteStaticMembers`. New APIs pop up that have static members in a class, and it can be useful to automatically have them in Intellisense without needing to write a static import first. For example, 2024/2025 added a new units API and a new `Units` class with static members for different units. That's something that could be nice to have in Intellisense. This is mostly personal preference though, so you don't have to add something if you don't want to, and adding too many things clutters up Intellisense.
- `java.completion.filteredTypes`. As of 2025, nothing has been added here, but if a class or package of classes keeps appearing in your Intellisense, and you want it gone, this is where to add it.
- `build.yml`. New versions of the actions used in the workflow come out occasionally, make sure the versions are updated from time to time.
