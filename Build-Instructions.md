# EDK II Build Instructions

Over the life of the project, EDK II has evolved it's build process. A common theme has been reducing the number of
manual steps involved and easing environment setup and configuration so developers can more quickly get started writing
firmware code.

There's currently three high-level approaches to build (listed in recommended order):

1. [[How to Develop With Containers]]
2. [[How to Build With Stuart]]
3. [build](https://github.com/tianocore/tianocore.github.io/wiki/Getting-Started-with-EDK-II)

## Build Option Comparison

Containers have seen widespread adoption in software development. The capability to deploy well-defined, ready-to-go
images, results in unmatched performance, portability, and consistency of build environments. By extension, TianoCore
leverages containers for both server-side builds (e.g. for pull requests and continuous integration) and for local
developer builds. The TianoCore project maintains containers in
[tianocore/containers](https://github.com/tianocore/containers).

If you just want to get started quickly and be able to receive the best support possible (since issues in containers
are easy to reproduce, fix, and deploy), then start with the container instructions.

Prior to containers, building involved a lot of manual steps. Downloading compilers, various dependencies, running
the right commands in the right order, and so on. A lot of that work was reduced and moved into a tool that
orchestrates a lot of the underlying steps needed to simply set up a build environment and start building code. That
tool is called "Stuart". So if you would like a local build environment without using containers, it is recommended
to use Stuart. Containers use Stuart and the CI system uses Stuart and many CI checks are performed by Stuart to allow
pull requests to be submitted. So running CI locally with Stuart will put you in a great position to have code ready
for contribution to the project.

At the core of the build is an application called `build`. Ultimately, containers and Stuart will eventually call
`build` to actually build the code and prior to the introduction of those two approaches, `build` was the primary
build path. So, you can still call `build` directly. This will result in more manual steps and a lack of the feature
set brought by the other two options but you can produce a working firmware image (in most cases) with `build`.
