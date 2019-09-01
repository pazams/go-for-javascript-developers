# Modules / Packages
## Spec & Practice
**JS**

As of es6, the Javascript spec includes a module system, however the external specs of AMD and CommonJS are also popular since the language began to address this issue rather late.

Before es6 modules, the spec only supported the *script* mode, of which every file shares the same top-level global scope. This means that there was no official "file scope" for scripts. In practice, file-module scope was common since it was either introduced by code (`window.moduleA = …`), an external tool (requireJS), or by a runtime that baked-in a module system (NodeJS).

Therefore, it is safe to say that Javascript programs are commonly structured with a 1-to-1 relationship between files and modules with local scope.

**Go**

Go's import statement and package support were part of the spec from the beginning. In Go there is no file scope, only package scope. As of Go 1.6 (or 1.5 + flag), there's better support for encapsulating dependent packages inside a project with the [vendor folder](https://blog.gopheracademy.com/advent-2015/vendor-folder/). However, it doesn't attempt to solve everything:
> … this does not attempt to solve the problem of vendoring resulting in multiple copies of a package being linked into a single binary. Sometimes having multiple copies of a library is not a problem; sometimes it is. At least for now, it doesn’t seem that the go command should be in charge of policing or solving that problem.

If choosing to use go modules, as introduced in Go 1.11, then it will make the vendor folder redundant by default (downloaded modules source code is stored at `$GOPATH/pkg/mod`). In exchange for the vendor folder, are `go.mod` and `go.sum` which together provide a 100% reproducible and secure builds (provided the means to fetch external modules). There's an [option](https://github.com/golang/go/wiki/Modules#how-do-i-use-vendoring-with-modules-is-vendoring-going-away) to keep using the vendor folder with modules.

**The differences**

A Javascript module can be any valid Javascript type. By exporting an object, it can *package* multiple functionalities. By exporting a function it can surface a single functionality. On the other hand, a Go package, is as its a name- just a package. So while a Javascript module can be directly invoked if it is a function type, this is not a possibility with a Go package.

Another difference is the consumption of other internal components within your project. In Javascript, since each file is (usually) a module, then each of the files that were decoupled from the current file must be imported. On the other hand, in Go, all files within the same package can have access to each other since there is no file scope.

## Management

For Javascript development, NPM is the defacto dependency manager for NodeJS, and may also be used for client-side projects. Bower is also a popular for client-side projects.

Go 1.11 introduced [go mod](https://github.com/golang/go/wiki/Modules), the official dependency manager.
As this was a late addition to the ecosystem, other community driven solutions are still in-use in some projects. Here is a partial list of these community solutions: 
- https://github.com/Masterminds/glide
- https://github.com/tools/godep
- https://github.com/kardianos/govendor
- https://github.com/mattn/gom
- https://github.com/golang/dep
