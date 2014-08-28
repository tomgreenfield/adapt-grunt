We have slightly adapted the build process from that used in the OSS project for use internally with multiple modules in a single course. Where possible, we have kept the name of the grunt tasks the same (or similar), however, it is important to check here regularly to keep up-to-date with any changes.

## Configuration

One major difference with our internal grunt build process is the addition of a `grunt_config.json` file. This file contains various config options needed by the Gruntfile which can be edited on a project-by-project basis.

See below for an annotated example:

    {
        // a list of all modules (any modules not specified here will not be built)
        "modules": [
      	    "m05",
      	    "m10",
      	    "m15"
        ],
        "themes": {
            // the default theme used by dev and build
      	    "default":"adapt-contrib-vanilla",
      	    // these override the default theme (needs to be specified per module)
      	    "custom": {
      		    "m15":"adapt-contrib-vanilla-alt"
      	    }
        }
    }


## Folder structure

The following section will outline the differences with the standard Adapt Learning folder structure and the CGKineo version.

**Grunt files**

The `Gruntfile.js` and the `grunt_config.json` both need to be placed in the course's root folder (internally, this is `dev`).

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

## Supported commands

Developing a course takes place in the `adapt_framework/src/courses` folder. When you want to test or release your creation, there are a few simple commands you should be aware of.

In a Node.js Command Prompt, navigate to the directory of the course (internally, this will be the dev folder). Before using any Grunt commands, you must run `npm install` to pull down the relevant packages used by Grunt. After NPM has installed the required plugins, there are several commands you can run.

**IMPORTANT NOTE: NOT ALL OF THE COMMANDS FROM THE OSS REPOSITORY HAVE BEEN PORTED. ANY GRUNT COMMAND NOT INCLUDED IN THE BELOW LIST IS UNSUPPORTED, AND THEREFORE SHOULD NOT BE USED**

The following grunt commands are armed and operational:

    grunt
Simply running `grunt` will list out the available tasks, along with colour coded instructions. It functions similarly to `grunt -h`, but with prettier formatting (it also has the benefit of ommitting any tasks that we don't support).

    grunt build:id
Builds a production ready/minified version of the specified module. Takes the module ID as a parameter. If no module
ID is specified, all modules are built. [Note: the id passed must also be present in the `grunt_config.json` or that module will not be built].

    grunt dev:id
Creates a developer-friendly version of the specified module (including source maps). Takes the module ID as a parameter. If no module ID is specified, all modules are built. If an individual module has been specified, it also runs the `watch`task for that module.

    grunt watch:id
Listens for changes to any files associated with the specified module, then performs the necessary actions to update the build.

    grunt tracking-insert:id
If your course is intended use SCORM to track within a LMS, you should run this command to insert tracking identifiers. The module ID is passed as a parameter.

    grunt server:id
Launches a stand-alone Node.JS web server and opens the specified course in your default web browser.

    grunt server-scorm:id
Same as above, but emulates a SCORM server to test the tracking of learner progress.


***Hint: if you forget which commands are available, you can run simply*** `grunt` ***to see a list of the supported commands (along with descriptions).***

## Creating the SCORM package

After running the `dev`/`build` tasks, a build of the relevant module will be created in the `builds` folder. Provided that no errors have occured, the contents of this folder can simply be zipped up to create a valid SCORM package.
