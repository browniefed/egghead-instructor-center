# Contributing

## Workflow

![Animated gif of pull request process](https://cloud.githubusercontent.com/assets/5497885/20947829/3bd6ce70-bbce-11e6-86a5-9df6e067c8cc.gif)

- Ensure you have Git, Node, and Yarn installed
- Clone this repo and `cd` into it
- `yarn` to install packages
- Create a feature branch off of `master`.
- `yarn dev` to run app with local dev endpoints ([egghead-rails needs to be running locally](https://gist.github.com/trevordmiller/35dcf0a705b8cb610178f18a135ea6e3))
- `yarn dev:staging` to run app with staging endpoints
- `yarn dev:prod` to run app with prod endpoints
- `yarn test` to run tests
- Open [localhost:3000](http://localhost:3000) to view app
- Log in with your egghead instructor account and environment password
- Submit a pull request to `master`.
- [Continuous integration](https://travis-ci.org/eggheadio/egghead-instructor-center) runs automatically.
- Merge pull request once continuous integration passes and you have an approved review.
- [Continuous deployment](https://app.codeship.com/projects/183842) pushes latest code to [prod](https://instructor.egghead.io) automatically.


## Conventions

### Package management

Yarn is used for package management. Use the [yarn cli](https://yarnpkg.com/en/docs/usage) to add/remove/update packages which updates both the `package.json` and `yarn.lock` to ensure consistent package installs. 

### Scripts

Yarn is used for running scripts. Use `yarn {script}` to run them. The core of the scripts extend `react-scripts` (from Create React App) so that this project doesn't have to manage compiling, linting, or testing; it generally gets all [Create React App updates](https://github.com/facebookincubator/create-react-app/releases) for free by running `yarn upgrade react-scripts`. Avoid ejecting the project for this reason if at all possible.

### Components

Each directory inside `src` is a **component**. A component is a directory organized _by feature_. They look like this:

```
SomeComponentName/
  index.js (entry point)
  components/ (optional sub-components)
  utils/ (optional utility modules)
```

### Screens

Some components are also **screens**. A screen is just a component that additionally corresponds with a route; it is generally a collection of `Card`s. They look like this:

```
SomeScreenName/
  index.js (entry point)
  screens/ (optional sub-screens paired with sub-routes)
  components/ (optional sub-components)
  utils/ (optional utility modules)
```

### Routing

[React Router](https://reacttraining.com/react-router/) is used for routing. URLs follow the Rails standard routing scheme, which uses plural entities named with a noun. For example:

```
/lessons
/lessons/new
/lessons/{id}
/lessons/{id}/edit
```

Routes correspond to screen components. For example, `src/screens/Lessons` is used for `/lessons` and `src/screens/Lessons/screens/new` is used for `/lessons/new`.

### Promotion

All resources are eligible for **promotion** to facilitate code reuse. If a resource is shared by multiple files the principle of [lowest common ancestor](https://en.wikipedia.org/wiki/Lowest_common_ancestor) applies and that shared resource is _promoted_ to the lowest common ancestor’s directory. A resource can be promoted further - outside of the project - into [`egghead-ui`](https://styleguide.egghead.io) or npm to be used across projects.

The idea is that when you create a new file or folder, you put it only where it's needed first (usually deep in the directory tree). Then, as other files need it you "promote" it (move the file or directory) up to the lowest common ancestor where it can be imported from multiple places below it. So, for example, when `LessonOverviews` was initially created, it was placed in `src/screens/Instructors/screens/Instructor/components/InstructorLessons/components/LessonOverviews`, but eventually, more screens needed `LessonOverviews`, so now it has been moved up to `src/components`. Other generic components that were originally inside individual component directories have now been promoted all the way to the `egghead-ui` project (like `Button`, `Icon`, `Loading`, `Request` etc.).

### Paths

ES2015 modules are used for sharing code between files. `NODE_PATH` is set to `src` so `import Icon from 'components/Icon'` will grab `src/components/Icon`. When trying to decide if an import should use an absolute or relative path, it depends on the situation: if something belongs to an inner module/component, it should reference the pieces relatively; if something is using a general promoted module/component, it should import the pieces absolutely. A good rule of thumb is to keep everything relative that would be moved together so it is self-contained.

### JavaScript

`latest` ES syntax is used where it makes sense. [Lodash](lodash.com) methods are often used in place of native methods for performance, convenience, and consistency.

### Styles

[`egghead-ui`](https://styleguide.egghead.io) components are used wherever possible. Styling is largely taken care of by these components. When app specific styles are needed beyond that, `tachyons-egghead` classes are used where possible. Styles are added to work on mobile first, the `*-ns` (not small) and `*-l` (large only) classes are appended to override/extend styles for large screen sizes.

### Endpoints

The [`WrappedRequest` component](https://github.com/eggheadio/egghead-instructor-center/blob/master/src/components/WrappedRequest/index.js) extends the [`Request` component](https://styleguide.egghead.io); it is used with the `egghead-rails` rest APIs, based on the current environment. The endpoint data uses hypermedia (urls to make requests to for sub-data instead of placing all data inline). Response properties and hypermedia urls are only shown when the user has access to them, so all UI based on permissions can be combined without separate routes for "roles". There is no such thing as roles in this system, but what data a response has, based on the user. This means that you can do something like `lesson.approve_url ? <ApproveButton /> : null` to show a lesson approval button and it will work for all users, regardless of their permissions.

### Public folder

The `public/` folder is used by `react-scripts` to copy files directly to the `build/` folder root. It contains things like the root `index.html` and favicons/app icons. It is generally read-only.

### Linting

Linting is currently provided by `react-scripts` (ESLint).

### Testing

Testing is currently provided by `react-scripts` (Jest). Files that could benefit from tests have an `index.test.js` file next to them. These are generally simple unit or snapshot tests where they provide value.

### Localization

Hard-coded strings are currently implemented with [react-localize](https://www.npmjs.com/package/react-localize) with the bundle in `src/utils/localizationBundle`.

### Continuous integration and deployment

[Travis CI](https://travis-ci.org/eggheadio/egghead-instructor-center) is currently used for continuous integration and deployment. It runs scripts to ensure builds pass before pull requests are merged into `master` and then deploys to Amazon S3 which updates [instructor.egghead.io](https://instructor.egghead.io).

### Error tracking

[Honeybadger](https://app.honeybadger.io/projects/51180/faults?q=-is%3Aresolved+-is%3Aignored) is currently used for error tracking.

### User tracking

[Google Analytics](https://analytics.google.com/analytics/web/?authuser=1#report/defaultid/a36512724w134681887p138806178/) is currently used for user tracking.
