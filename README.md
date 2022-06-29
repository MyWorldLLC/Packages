# MyWorld Open Source Packages
The [MyWorld Project](https://myworldvw.com) publishes a growing number of open source libraries. This repository is a placeholder for publication of Maven artifacts
via GitHub Packages.

To use these artifacts in your Gradle builds, add the following to your build script:
```groovy
def getGitCredentials(){
    def process = "git credential fill".execute()
    def stream = new java.io.PrintStream(process.getOutputStream(), true)
    stream.println("url=https://github.com/")
    stream.close()

    process.text.trim().split("\n").collectEntries {it.split("=")}
}

gitCredentials = getGitCredentials()
repositories {
  maven {
		url 'https://maven.pkg.github.com/MyWorldLLC/Packages'
		credentials {
			username System.getenv("GITHUB_USER") ?: gitCredentials["username"]
			password System.getenv("GITHUB_TOKEN") ?: gitCredentials["password"]
		}
	}
}
```

The `getGitCredentials()` helper function can be replaced with any other mechanism you like, and will be overridden by supplying the 
`GITHUB_USER` and `GITHUB_TOKEN` environment variables. See 
[GitHub's docs](https://docs.github.com/en/packages/learn-github-packages/introduction-to-github-packages#authenticating-to-github-packages)
for help with authentication issues.
