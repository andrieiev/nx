# Using Jest

![Jest logo](../../shared/jest-logo.png)

[Jest](https://jestjs.io/) is an open source test runner created by Facebook. It is used within Facebook internally as well as many other enterprise and open source projects including Nx itself!

## Reasons for Using Jest

- Jest is faster than karma allowing more time to be spent developing features rather than validating them.
- Jest was built with monorepos in mind and is able to isolate the important parts of a monorepo to test.
- Jest has a great built-in reporter for printing out results of tests.
- Jest has an immersive watch mode which provides near instant feedback when developing tests.
- Jest provides the ability to use Snapshot Testing to validate features.
- And more...

## How to use Jest

By default, Nx will use Jest when creating applications and libraries.

```treeview
<workspace name>/
├── README.md
├── angular.json
├── apps/
│   ├── frontend/
│   │   ├── browserslist
│   │   ├── jest.conf.js # <= jest config
│   │   ├── src/
│   │   │   ├── app/
│   │   │   ├── assets/
│   │   │   ├── environments/
│   │   │   ├── favicon.ico
│   │   │   ├── index.html
│   │   │   ├── main.ts
│   │   │   ├── polyfills.ts
│   │   │   ├── styles.scss
│   │   │   └── test.ts
│   │   ├── tsconfig.app.json
│   │   ├── tsconfig.json
│   │   ├── tsconfig.spec.json
│   │   └── tslint.json
│   └── frontend-e2e/
├── libs/
├── nx.json
├── package.json
├── tools/
├── tsconfig.json
└── tslint.json
```

Older versions of Nx used Karma as a default test runner. For those workspaces, you have provide the `--unit-test-runner=jest` option when creating applications or libraries.

### Running Tests

```bash
ng test frontend
```

### Snapshot Testing

Jest has support for **Snapshot Testing**, a tool which simplifies validating data did not change. Check out the [official Jest Documentation on Snapshot Testing](https://jestjs.io/docs/en/snapshot-testing).

#### Writing Tests Using Snapshot Testing

To write a test which uses **Snapshot Testing**, use the `toMatchSnapshot()` matcher.

```typescript
describe('Home Page', () => {
  it('should have a header', () => {
    const header = fixture.nativeElement.querySelector('header');
    expect(header).toMatchSnapshot();
  });
});
```

The snapshot will be generated the first time the test is run. If the contents of that snapshot change, the test will fail indicating unexpected changes to the snapshot. Below is an example of the test results if the hamburger icon disappears unintentionally.

```bash
Home Page > should have a header
 expect(value).toMatchSnapshot()
 Received value does not match stored snapshot "Home Page should have a header 1".
 - Snapshot
+ Received
 <header>
  <h1>
-   <mat-icon>
-     hamburger
-   </mat-icon>
    Example
  </h1>
</header>
```

> Note: These snapshot files should be checked in with your code.

#### Updating Snapshots

When intentionally changing the contents of a snapshot, you can run tests with the `--updateSnapshot` flag to update failing snapshots instead of failing the test.

```bash
ng test libname --updateSnapshot
```

> Make sure no **unintentional** snapshots are failing **BEFORE** updating failing snapshots.

### Watching for Changes

If you are a developer making changes locally to a library, start jest's interactive watch mode to run the library's tests related to uncommitted changes and then rerun tests whenever files are changed.

```bash
ng test libname --watch
```

#### Debugging Failing Tests

To debug failing tests using Chrome Devtools or an IDE you can run the test command through node's `--inspect-brk` flag:

```bash
node --inspect-brk ./node_modules/@angular/cli/bin/ng test libname
```

Now, you can visit [chrome://inspect](chrome://inspect) in Chrome and inspect the target to attach to the node process. You can now use Chrome Devtools to step through your code line by line and debug the cause of the failing tests. Visit the official [Jest documentation](https://jestjs.io/docs/en/troubleshooting#tests-are-failing-and-you-don-t-know-why) to find out more.
