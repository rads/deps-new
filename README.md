# deps-new

The next generation of `clj-new`. Work-in-progress!

Intended to be installed as a "tool" (Clojure CLI 1.10.3.933 or later).

```bash
clojure -Ttools install com.github.seancorfield/deps-new '{:sha "..."}' :as deps-new

clojure -Tdeps-new app :name myusername/mynewapp
```

Creates a directory `mynewapp` containing a new application project, with `myusername` as the
"top" namespace and `mynewapp` as the main project namespace:

```clojure
;; mynewapp/src/myusername/mynewapp.clj
(ns myusername.mynewapp
  (:gen-class))

(defn greet
  "Callable entry point to the application."
  [data]
  (println (str "Hello, " (or (:name data) "World") "!")))

(defn -main
  "I don't do a whole lot ... yet."
  [& args]
  (greet {:name (first args)}))
```

```bash
clojure -Tdeps-new lib :name myusername/mycoollib
```

Creates a directory `mycoollib` containing a new library project, with `myusername` as the "top" namespace
and `mycoollib` as the main project namespace under that.

If you want to generate the project into a different directory than the project name, use
the `:target-dir` option to specify a path to the directory that should be created:

```bash
clojure -Tdeps-new lib :name myusername/mycoollib :target-dir projects/newlib
```

Creates a directory `projects/newlib` containing a new library project, with `myusername` as
the "top" namespace and `mycoollib` as the main project namespace under that.

If you just want a very minimal `deps.edn` project to experiment with:

```bash
clojure -Tdeps-new scratch :name play
```

Creates a directory `play` containing an empty `deps.edn` file and `src/scratch.clj`
with a simple `exec` function (you can invoke via `clojure -X scratch/exec`) and a
simple `-main` function (you can invoke via `clojure -M -m scratch`). This is intended
to be a minimal "playground" to get started with `deps.edn` and the CLI.

Currently those are only the only three built-in templates (`app`, `lib`, and `scratch`).

More general usage:

```bash
clojure -A:somealias -Tdeps-new create :template com.acme.project/cool-lib :name myusername/mynewproject
```

Looks for `com/acme/project/cool_lib/template.edn` on the classpath (based on the `:somealias` alias) and,
if present, uses that template to create a project, in `mynewproject`. Instead of `-A:somealias`, you
could use `-Sdeps` to specify the dependencies needed to make the template available.

See [**Project Names and Variables**](doc/names-variables.md) to see how the project name (`:name`)
is used to derive the default values of all the built-in substitution variables.
See [**All the Options**](doc/options.md) for the full list of command-line options available
when invoking `deps-new`. See [**Writing Templates**](doc/templates.md) for documentation on
how to write your own templates.

# License

Copyright © 2021 Sean Corfield

Distributed under the Eclipse Public License version 1.0.
