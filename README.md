# Introduction to UI Tests

UI Tests can be broadly classified into two major categories

1. **Structural Tests**
2. **Visual Tests**

## 1. Structural Tests

Unit tests are written using Jest, react-testing-library which renders react component on nodejs server and helps perform interaction with the component. With structural testing, we can only be functionally sure about what className and what div structures gets rendered. Jest's Snapshot feature help us save the same and compare the structure of UI component.
As, we would like to achieve 100% code coverage in these tests and Jest reports is quite helpful to figure out code snippet covered during test run.

### Contribution of Structural tests for UI component

1. Start by creating file under ComponentName Folder as, `${ComponentName}.spec.js` or `${ComponentName}.test.js`
2. Test suite will start with `describe` function and test will as `test` function accepting callback function as test code.
3. Run unit tests with command `npm run test:unit`.
4. Reports will be generated `jest-start/index.html`.
5. Make sure to analyse Code Coverage report to focus on writing concrete tests which will assure 100% coverage. Coverage report will be found here `coverage/lcov-report/index.html`.

## Visual Tests

Visual tests operate by taking initial Base Image Snapshot when we write Test in the beginning. During subsequent test run, will compare the newly taken snapshot against the stored baseline. This helps us do testing at the pixel level.

## Working

Here we primarily test the actual view of the UI component in the browser environment. `Cypress` is used to automate visit to the page where component is rendered and to perform interactions. `cypress-image-snapshot` plugin is used to capture and compare image snapshot.

### Storybook to the rescue

As we need to render UI component to the browser, we need some web dever server setup for same. Storybook helps here, which runs it's own web-dev server and host the stories written inside `${ComponentName}.stories.js`
Storybook is a user interface development environment and playground for UI components. Storybook enables developers to create components independently and helps interact in an isolated development environment.

**Benefits of storybook**

- Supports interaction with these component in the browser using storybook-addon `withKnobs`. We will add separate story with name `usage`. We also add `withInfo` plugin to this `usage` story. This automatically pulls documentation for the component.
- `Show Info` button will help us read details about the component in storybook window
- `Knobs` tab in the panel, exposes the various props to interact with the component.
- `Actions` tab in the panel, display message/event object based on the event triggered.

**IFrame url for the component in the storybook**
For our testing, we would be directly accessing iframe url from storybook server.
sample url `http://localhost:9009/iframe.html?id=common-badge--default`

\*\* we write `usage` story seperately and will not be using `usage` url in our visual testing, as it has documentation `withInfo` decorator, which repeats our elements on the page. Hence accessing element fails in usage-iframe url.

We can run storybook using below command
`/> npm run storybook`
This will start storybook on the port 9009. `http://localhost:9009`

### Contribution of Visual tests for UI component

1. Start by creating file under ComponentName Folder as, `${ComponentName}.visual.spec.js` or `${ComponentName}.visual.test.js`
2. Test suite will start with `describe` and tests will as `it` accepting callback function as test code.
3. Run tests with command `npm run test:visual`.
4. Reports will be generated `mochawesome-report/mochawesome.html`.
5. Make sure to analyse Code Coverage report to focus on writing concrete tests which will assure 100% coverage.

## How to write story for testing

We would follow below patterns while we write stories

- `default` story : without any additional props set; how will our component render with minimal input set?
- `props` stories : We can pick each prop and host it's own story page with different values. for example, `http://localhost:9009/iframe.html?id=common-button--appearance` and `http://localhost:9009/iframe.html?id=common-button--size`
- `action props` based story : We can host story page where we would perform some interaction and then capture before and after visual. for example, `http://localhost:9009/iframe.html?id=common-tooltip--position`

### How to update snapshot

Sometimes we need to update the snapshot. Use below command with proper spec file to update.
`/> npm run test:cypress:updateSnapshot -- --spec=lib/components/Badge/Badge.visual.spec.js`

### known issue.

When running tests on Monitor with different resolution, mage snapshot may fail to match against baseline as baseline Image was captured on Mac laptop of different resolution.
[resolution issue](https://github.com/cypress-io/cypress/issues/3324)
