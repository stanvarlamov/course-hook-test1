### package.json and package-lock.json

`package.json` is written to [NodeJS Package Manager standard](https://docs.npmjs.com/creating-a-package-json-file). Sure enough, it is also usually copied from somewhere initially and then updated. The `package-lock.json` is auto-generated or updated when the Package Manager runs the installation and brings in all required 3rd party ("external") packages, listed in the `dependencies` and `devDependencies` sections of `package.json`. Just like with other systems, e.g., `Maven` in Java or `pip` in Python, correct dependencies management is absolutely crucial for your custom project. 

Stable release versions of external packages are usually 3 dot-separated numbers. The dependency list must specify which version should be loaded by the Package Manager into `node_modules/` and used and runtime. As the external packages have their own dependencies, the Package Manager applies some logic to ensure that all required dependencies are brought in. Reusable common dependencies and everything listed in your `package.json` is placed into individual sub-folders under `node_modules`. If an external package requires a version of some package different from the one listed in `package.json`, that version will be loaded into a separate `node_modules` sub-folder under the external package's sub-folder - for its own use.

You can use version-matching rules to define if you require the exact 3-pos "Patch" version, or any Patch within the 2-pos "Minor" release, or anything in the 1-pos "Major" release. The rules can be coded using `< = >` signs and/or tilde or caret `~^` in front of the numbers, as well as `*` for any version, including unstable, or the word `latest` for the latest available stable version of the package. The tilde `~` generally means the exact match of the listed numbers. The caret `^` can be confusing as its logic is based on zero vs. non-zero numbers in the combination it prefixes. In its common use, the dev places `^` in front of the current latest available Patch version, and that guarantees that the package will be loaded at least at that version or at a later Minor-Patch, but at the same Major. The latest available package version can be found by going to the package's npm site or prompted by the IDE when it is connected to the npm system over the internet.

Once the logic is applied, the resulted combination of packages and versions loaded is written into the lock file `package-lock.json` (or `yarn-lock.json`). Theoretically, if the lock file is present, the Package Manager will load the exact structure as written in it, therefore creating repeatable installations with matching content of `node_modules/`.

So, the question is what logic should you apply when defining external package version requirements? Basically, if you are too strict on the versioning (require the match at the Patch level) - there is a greater chance in the lifecycle of your project that some external packages will require dependencies higher than what you require for your project, and therefore those dependencies would need to be loaded into individual external packages' sub-folders, growing the size of the overall footprint. If, on the other extreme, you allow loading the latest versions of your dependencies, there is a risk that when you delete the lock file or the Package Manager regenerates the dependencies structure when you change something on your list - you'll get an external package update that breaks your code.

Different systems periodically inform you if you vulnerabilities have been identified in external packages your project has loaded - in this case you need to review your `package-lock.json` and then fix the underlying cause in `package.json`. Otherwise, you would probably leave the dependencies as-is after the initial development. So, a good approach would be getting the latest versions from the start and locking them in your `package.json` at the Major level by prefixing everything with a `^`. 

Packages listed in the `devDependencies` section are only required for the dev environment and are not loaded when the production environment is built.

We will review the content of `package.json` used in this course in details as we explain individual JS code components and processes