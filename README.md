We have slightly adapted the build process from that used in the OSS project for use internally with multiple modules in a single course. Where possible, we have kept the name of the grunt tasks the same (or similar), however, it is important to check here fairly regularly to keep up-to-date with any changes.

## Configuration

One major difference with our internal grunt build process is the addition of a `grunt_config.json` file. This file contains various config options needed by the Gruntfile which can be edited on a project-by-project basis.

See the `grunt_config.json` in this repo for an example configuration.

### Modules

This is a list of all modules in the project. Any modules not listed here will not be built when using the `buildall` or `devall` commands. Similarly, the `build` and `dev` commands will **not** function if a module ID is passed which isn't present in this list.

### Themes

This contains various nested data which is relevant to the module themes in the course..

**themes.default**: the default theme used when calling any of the build/dev tasks. This will be used unless a custom theme is specified (see below).

**themes.custom**: any modules which have a custom theme (i.e. not that specified in default) need to be listed here in the format: `"moduleID":"theme-name"`

## Folder structure

The following section will outline the differences with the standard Adapt Learning folder structure and the CGKineo version.

**Grunt files**

The `Gruntfile.js` and the `grunt_config.json` both need to be placed in the course's root folder (which should be `dev` in most cases).

**Builds**

As there can now be multiple modules/builds per course, the `build` folder has now become `builds`.

**Course data**

The course data for each module needs to be placed in a `courses` folder in the `src` for the project. Each course then needs to be named with it's module id (to correspond with the list in `grunt_config.json`). For example:

    src/
        courses/
            m05/
            m10/
            m15/
            ...
        ...

**Themes**

In order for multiple themes to work within a single course, there has had to be a small change to their folder structure. The theme data now needs to be nested within a `src` folder.

So the following:

    theme-name/
        assets/
        fonts/
        js/
        ...

Becomes:

    theme-name/
        src/
            assets/
            fonts/
            js/
            ...

## Supported commands

Developing a course takes place in the `adapt_framework/src/courses` folder. When you want to test or release your creation, there are a few simple commands you should be aware of.

In a Node.js Command Prompt, navigate to the `adapt_framework` directory. From here there are several commands you can run.

Not all of the commands from the OSS Gruntfile have been ported across to the CGKineo internal fork.

**ANY GRUNT COMMAND NOT INCLUDED IN THE BELOW LIST IS UNSUPPORTED, AND THEREFORE SHOULD NOT BE USED**

The following grunt commands are armed and operational:

    grunt buildmod:id
Builds a single production ready/minified version of the specified module. Takes the module id as a parameter. Mimics the build command found in OSS Adapt. [Note: the id passed must also be present in the `grunt_config.json` or an error will be thrown].


    grunt buildall
Builds all modules. Uses list of modules in the `grunt_config.json` file.


    grunt devmod:id
Creates a developer-friendly version of the specified module (including source maps). Takes the module ID as a parameter. Also runs the `watch` task, which watches the course `src` folder for any updates, and performs the relevant grunt tasks to update the build.


    grunt devall
Runs the `devmod` task on all modules. Does NOT run the `watch` task.


    grunt tracking-insert:id
If your course is intended use SCORM to track within a LMS, you should run this command to insert tracking identifiers. The module ID is passed as a parameter.

    grunt server:id
Launches a stand-alone Node.JS web server and opens the specified course in your default web browser.


    grunt server-scorm:id
Same as above, but emulates a SCORM server to test the tracking of learner progress.


Although the following are still functional, the alternative grunt tasks outlined above should be used instead.

    grunt build
    grunt dev
    grunt server
    grunt server-scorm
These commands have been left for legacy reasons. Calling any of these will result in the equivalent new grunt task being run on the first module specified in the `grunt_config.json`. For example: `grunt build` equates to `grunt buildmod:m05` (assuming that m05 is the first module listed in the `grunt_config.json`).


## Creating the SCORM package

After running the dev/build tasks, a build of the relevant module will be created in the `builds` folder. Provided that no errors have occured, the contents of this folder can simply be zipped up to create a valid SCORM package.
