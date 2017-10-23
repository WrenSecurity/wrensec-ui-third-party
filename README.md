# Wren Security Third-party UI Libraries
This project contains all of the JavaScript and/or CSS Libraries that are
provided by third-parties outside ForgeRock + Wren Security.

## How This Repository is Used
This repository merely wraps existing libraries in a POM package, to unify
management of dependencies between Wren Security projects and JS packages.

Previously, ForgeRock imported JavaScript and CSS libraries into their
Artifactory instance directly, and auto-generated POMs in the process. There
were three problems with this approach:

1. It did not allow artifacts to be signed with a GPG key (since nothing in the
   standalone JS/CSS artifact publishing process would cause PGP signing to
   occur).
2. It did not allow for PGP signature enforcement (since the generated POM did
   not extend from a POM that enforced signatures).
3. It did not consistently expose the project name, project description, and
   project licensing information in the auto-generated POM (most projects had a
   description of "Artifactory auto generated POM" or "POM was created from 
   install:install-file").

## Adding New Artifacts to This Project
A Maven archetype has been provided as part of this project that streamlines
the process of adding new libraries. The archetype is used to generate a
"wrapper project" for a new library.

### Installing the Archetype
The archetype is installed locally like any other Maven artifact, as follows:

1. `cd ui-third-party-archetype/`
2. `mvn clean install`

### Generating a New Wrapper Project and Adding Library Files
Follow these steps to add the new library to this project:

1. From the root of this project (i.e. under `wrensec-ui-third-party`), run the
   following command:
   ```
   mvn archetype:generate -DarchetypeGroupId=org.wrensecurity.commons.ui.libs \
     -DarchetypeArtifactId=ui-third-party-archetype \
     -DgroupId=org.wrensecurity.commons.ui.libs -Dversion=1.0-SNAPSHOT
   ```
2. Follow the prompts and provide relevant answers for the library you are
   adding. Most of the information typically comes from the library's project
   information on GitHub. Be sure to answer `Y` at the end to create the
   wrapper project.
3. Copy the minified / release-grade JS and/or CSS files for the library into
   the `LIBRARY-NAME/VERSION` folder.
4. Customize the POM found at `LIBRARY-NAME/pom.xml`, where `LIBRARY-NAME` is
   the name of the library project you created. Make the following changes:
   1. If the names of the JavaScript files for the library do not end in
      `-min.js`, customize the `jsPackageFiles` and `jsClassifier` properties
      appropriately.
   2. If the library not contain any JavaScript files, remove the
      `jsPackageFiles` and `jsClassifier` properties completely.
   3. If the names of the CSS files for the library do not end in `-min.css`,
      customize the `cssPackageFiles` and `cssClassifier` properties
      appropriately.
   4. If the library not contain any CSS files, remove the `cssPackageFiles`
      and `cssClassifier` properties completely.
5. Customize the POM found at `LIBRARY-NAME/VERSION/pom.xml`, where
   `LIBRARY-NAME` is the name of the library project you created, and `VERSION`
   is the version of the library you are adding. Make the following changes:
   1. If the library not contain any JavaScript files, remove the line that
      reads: `<skipJs>false</skipJs>`.
   2. If the library not contain any CSS files, remove the line that
      reads: `<skipCss>false</skipCss>`.
6. From the root of this project (i.e. under `wrensec-ui-third-party`), run the
   following command:
   ```
   mvn clean install
   ```
7. Two possibilities:
   - **If the build passes:** inspect the output from Maven to confirm that the
   JavaScript and CSS files were installed into the local repository, as
   expected.
   - **If the build fails:** inspect the error output and the generated POM
   files to confirm that everything looks as expected, and that filename
   patterns match the names of the library files added to the project.
   
# License
The contents of this file and all POM files in this project are subject to the
terms of the Common Development and Distribution License (the License). You may
not use this file except in compliance with the License.

You can obtain a copy of the License at legal/CDDLv1.0.txt. See the License
for the specific language governing permission and limitations under the
License.

When distributing Covered Software, include this CDDL Header Notice in each
file and include the License file at legal/CDDLv1.0.txt. If applicable, add
the following below the CDDL Header, with the fields enclosed by brackets []
replaced by your own identifying information: "Portions copyright [year]
[name of copyright owner]".

Copyright 2017 Wren Security.
