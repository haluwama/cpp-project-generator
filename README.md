# C++ Project Generator

This is primarily for personal use, though feel free to use it youself as well.

Often when creating new C++ projects, I would end up copy and pasting CMakeLists.txt files and build scripts, manually having to change project names and other mundane activies. To mitigate this issue, I created this project, which would create these project files for me.

## Building

On linux

```bash
sh build.sh release
```

## Configuartion

### Setting up

The project makes some assumptions about your C++ workspace structure.

* You have a root folder for your projects
* You have a folder for console/terminal apps to put the generator into

For example, on my system, I have this:

```sh
├── cpp-projects
    └── terminal
        └── cpp-project-generator
```

My C++ projects are in the `cpp-projects` directory, and the project generator is in the `cpp-projects/terminal/cpp-project-generator` directory.

To create the generator, `cd` into `cpp-project-generator` and run:

```bash
sh build.sh release
```

After building, your cpp-projects folder will have the following structure:

```sh
└── cpp-projects
    ├── new
    ├── templates
    |   ├── build.sh
    |   ├── debug.sh
    |   ├── deploy.sh
    |   ├── run.sh
    |   ├── opengl
    |   ├── sfml
    |   └── terminal
    └── terminal
        └── cpp-project-generator
```

This created the `new` executable, which can be used to create projects.

It also created a `templates` directory. This is where scripts for your new projects will be copied from, as well as where the project template layouts are stored. For example, SFML projects use the layout from the templates/sfml folder, and then copy the scripts into a scripts folder. More info in text section.

The scripts:

* `build.sh` - Used to build your projects. Use `build.sh release` to build in release mode.
* `run.sh` - Used to run your projects. Use `run.sh release` to run in release mode.
* `debug.sh` - Used to run GDB to debug your project.
* `deploy.sh` - Used to create a directory with the project executable and the resources folder (if there is one)

### Using

To use, `cd` to the `cpp-projects` folder and then:

`./new <Project type> <Project name>`

* Project type: The project template to use, eg terminal or sfml. This can be any directory in the projects/ folder
* Project name: The name of the project to create, eg my-app

EG

`./new terminal my-app`

#### Adding new templates

To add a template, simply create a folder in the cpp-project-generator/templates/ directory, inside including all the files you need for that template.

You must provide a CMakeLists file, where the project name is <PNAME>, which this program will auto replace with the name chosen on project create.

Rerun `sh build.sh release` to update the templates folder in the `cpp-projects` directory.

#### Example usage

```bash
./new terminal my-application
```

This will create a project folder `cpp-projects/terminal/my-app/` with the structure

```sh
└── my-app
    ├── CMakeLists.txt
    ├── scripts
    │   ├── build.sh
    │   ├── debug.sh
    |   ├── deploy.sh
    │   └── run.sh
    └── src
        └── main.cpp
```

After doing `cd` to the directory, it can then be built and ran:

```bash
sh scripts/build.sh
sh scripts/run.sh
```

After the build stage, it creates the following structure:

```
└── my-app
    ├── bin
    │   ├── debug
    │   │   ├── CMakeCache.txt
    │   │   ├── CMakeFiles
    │   │   ├── cmake_install.cmake
    │   │   ├── Makefile
    │   │   └── new
    │   └── release
    ├── CMakeLists.txt
    ├── scripts
    │   ├── build.sh
    │   ├── debug.sh
    |   ├── deploy.sh
    │   └── run.sh
    └── src
        └── main.cpp
```

Where the target was built in the `bin/debug/` directory.

To build and run in release mode, use the `release` argument:

```bash
sh scripts/build.sh release
sh scripts/run.sh release
```

