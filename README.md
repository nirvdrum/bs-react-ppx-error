# ReasonReact / BuckleScript 7.1.0+ Build Error

This project is little more than the generated files from the `react-hooks` along with the `reason-apollo` dependency.
I created this project to help illustrate a compilation issue introduce with the new ReasonReact PPX shipping with BuckleScript 7.1.0.
This project is illustrative of what new users to the ReasonML ecosystem, hoping to get started with ReasonReact and GraphQL, are likely to run into.

With BuckleScript 7.1.1, `yarn build` will fail with:

```
➜ yarn build
yarn run v1.21.1
$ bsb -make-world
Different compiler version: clean current repo
Cleaning... 46 files.
[37/37] Building src/ReactDOMRe.cmj
Different compiler version: clean current repo
Cleaning... 48 files.
[29/36] Building src/graphql-types/ReasonApolloQuery.cmj
FAILED: src/graphql-types/ReasonApolloQuery.cmj src/graphql-types/ReasonApolloQuery.cmi /home/nirvdrum/dev/workspaces/my-react-app/node_modules/reason-apollo/src/graphql-types/ReasonApolloQuery.bs.js
/home/nirvdrum/dev/workspaces/my-react-app/node_modules/bs-platform/lib/bsc.exe -nostdlib -bs-package-name reason-apollo  -bs-package-output commonjs:src/graphql-types -color always -bs-suffix -I src/graphql-types -I src -I /home/nirvdrum/dev/workspaces/my-react-app/node_modules/reason-react/lib/ocaml -I /home/nirvdrum/dev/workspaces/my-react-app/node_modules/bs-platform/lib/ocaml -w a -bs-super-errors -o src/graphql-types/ReasonApolloQuery.cmj src/graphql-types/ReasonApolloQuery.reast

  We've found a bug for you!
  /home/nirvdrum/dev/workspaces/my-react-app/node_modules/reason-apollo/src/graphql-types/ReasonApolloQuery.re 168:10-29

  166 ┆ let make =
  167 ┆     (
  168 ┆       ~variables: Js.Json.t=?,
  169 ┆       ~pollInterval: int=?,
  170 ┆       ~notifyOnNetworkStatusChange: bool=?,

  This pattern matches values of type
    Js.Json.t (defined as Js.Json.t)
  but a pattern was expected which matches values of type
    option('a)

[30/36] Building src/graphql-types/ReasonApolloMutation.cmj
FAILED: src/graphql-types/ReasonApolloMutation.cmj src/graphql-types/ReasonApolloMutation.cmi /home/nirvdrum/dev/workspaces/my-react-app/node_modules/reason-apollo/src/graphql-types/ReasonApolloMutation.bs.js
/home/nirvdrum/dev/workspaces/my-react-app/node_modules/bs-platform/lib/bsc.exe -nostdlib -bs-package-name reason-apollo  -bs-package-output commonjs:src/graphql-types -color always -bs-suffix -I src/graphql-types -I src -I /home/nirvdrum/dev/workspaces/my-react-app/node_modules/reason-react/lib/ocaml -I /home/nirvdrum/dev/workspaces/my-react-app/node_modules/bs-platform/lib/ocaml -w a -bs-super-errors -o src/graphql-types/ReasonApolloMutation.cmj src/graphql-types/ReasonApolloMutation.reast

  We've found a bug for you!
  /home/nirvdrum/dev/workspaces/my-react-app/node_modules/reason-apollo/src/graphql-types/ReasonApolloMutation.re 133:10-29

  131 ┆ let make =
  132 ┆     (
  133 ┆       ~variables: Js.Json.t=?,
  134 ┆       ~onError: apolloError => unit=?,
  135 ┆       ~onCompleted: unit => unit=?,

  This pattern matches values of type
    Js.Json.t (defined as Js.Json.t)
  but a pattern was expected which matches values of type
    option('a)

[31/36] Building src/graphql-types/ReasonApolloSubscription.cmj
FAILED: subcommand failed.
Failure: /home/nirvdrum/dev/workspaces/my-react-app/node_modules/bs-platform/lib/ninja.exe     Location: /home/nirvdrum/dev/workspaces/my-react-app/node_modules/reason-apollo/lib/bs       error Command failed with exit code 2.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

With BuckleScript 7.0.1, the build succeeds:

```
➜ yarn add bs-platform@7.0.1
yarn add v1.21.1
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
warning "bs-platform" is already in "devDependencies". Please remove existing entry first before adding it to "dependencies".
success Saved 1 new dependency.
info Direct dependencies
└─ bs-platform@7.0.1
info All dependencies
└─ bs-platform@7.0.1
Done in 8.97s.

➜ yarn clean; yarn build
yarn run v1.21.1
$ bsb -clean-world
Cleaning... 46 files.
Cleaning... 48 files.
Cleaning... 7 files.
Done in 0.54s.
yarn run v1.21.1
$ bsb -make-world
[37/37] Building src/ReactDOMRe.cmj
[36/36] Building src/ReasonApollo.cmj
[22/22] Building src/Index-ReasonReactExamples.cmj
Done in 1.43s.
```

Note that the error originates in the [reason-apollo](https://github.com/apollographql/reason-apollo) dependency.
reason-apollo uses an optional argument with a supplied type and this construct has been buggy with ReasonReact, so ReasonReact made a change to raise an error if such signatures exist in any ReasonReact component definitions.
Note also that the project's `reason-react` dependency doesn't change, only the `bs-platform` dependency does, which can be confusing as the error emanates from ReasonReact.

## Run

```sh
npm install
npm run server
# in a new tab
npm start
```

Open a new web page to `http://localhost:8000/`. Change any `.re` file in `src` to see the page auto-reload. **You don't need any bundler when you're developing**!

**How come we don't need any bundler during development**? We highly encourage you to open up `index.html` to check for yourself!

# Features Used

|                           | Blinking Greeting | Reducer from ReactJS Docs | Fetch Dog Pictures | Reason Using JS Using Reason |
|---------------------------|-------------------|---------------------------|--------------------|------------------------------|
| No props                  |                   | ✓                         |                    |                              |
| Has props                 |                   |                           |                    | ✓                            |
| Children props            | ✓                 |                           |                    |                              |
| No state                  |                   |                           |                    | ✓                            |
| Has state                 | ✓                 |                           |  ✓                 |                              |
| Has state with useReducer |                   | ✓                         |                    |                              |
| ReasonReact using ReactJS |                   |                           |                    | ✓                            |
| ReactJS using ReasonReact |                   |                           |                    | ✓                            |
| useEffect                 | ✓                 |                           |  ✓                 |                              |
| Dom attribute             | ✓                 | ✓                         |                    | ✓                            |
| Styling                   | ✓                 | ✓                         |  ✓                 | ✓                            |
| React.array               |                   |                           |  ✓                 |                              |

# Bundle for Production

We've included a convenience `UNUSED_webpack.config.js`, in case you want to ship your project to production. You can rename and/or remove that in favor of other bundlers, e.g. Rollup.

We've also provided a barebone `indexProduction.html`, to serve your bundle.

```sh
npm install webpack webpack-cli
# rename file
mv UNUSED_webpack.config.js webpack.config.js
# call webpack to bundle for production
./node_modules/.bin/webpack
open indexProduction.html
```

# Handle Routing Yourself

To serve the files, this template uses a minimal dependency called `moduleserve`. A URL such as `localhost:8000/scores/john` resolves to the file `scores/john.html`. If you'd like to override this and handle URL resolution yourself, change the `server` command in `package.json` from `moduleserve ./ --port 8000` to `moduleserve ./ --port 8000 --spa` (for "single page application"). This will make `moduleserve` serve the default `index.html` for any URL. Since `index.html` loads `Index.bs.js`, you can grab hold of the URL in the corresponding `Index.re` and do whatever you want.

By the way, ReasonReact comes with a small [router](https://reasonml.github.io/reason-react/docs/en/router) you might be interested in.
