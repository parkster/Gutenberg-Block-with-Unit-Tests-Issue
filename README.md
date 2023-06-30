Gutenberg Block with Unit Tests Issue
======
## Steps to recreate ##

**Install via wp create-block**
```bash
npx @wordpress/create-block gutenberg-block-unit-tests --wp-scripts
```

**Add unit test script to ```package.json```**
```json
"test:unit": "wp-scripts test-unit-js"
```

**Add sample ```utils/index.js``` file**
```javascript
export function add( a, b ) {
    return a + b;
}
```

**Add ```__test__``` folder to ```src```, add sample ```add.test.js```**
```javascript
import { add } from "../utils";

test("return addition", () => {
  expect(add(1,1)).toBe(2);
});
```

**Run unit tests**
```bash
npm run test:unit
```

👍 all good

```bash
 PASS  src/__test__/add.test.js
  ✓ return addition (1 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.387 s
Ran all test suites.
```

**Create test for edit component ```edit.test.js```**

```javascript
import render from '@testing-library/react';

import Edit from '../edit';

describe( 'Renders block', () => {
    return render(
        <Edit />
    );
} );
```

**Failure:**
```
 FAIL  src/__test__/edit.test.js
  ● Test suite failed to run

    Cannot find module '@testing-library/react' from 'src/__test__/edit.test.js'

    > 1 | import render from "@testing-library/react";
        | ^
      2 |
      3 | import Edit from "../edit";
      4 |

      at Resolver._throwModNotFoundError (node_modules/jest-resolve/build/resolver.js:427:11)
      at Object.require (src/__test__/edit.test.js:1:1)

Test Suites: 1 failed, 1 passed, 2 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.717 s
Ran all test suites.
```
** So then I add the package `npm install @wordpress/jest-preset-default --save-dev
`**
**...then add ```jest-preset-default``` > ```jest.config.json```**
```json
{
    "preset": "@wordpress/jest-preset-default"
}
```

**Error is removed, but then another error appears on both tests:**
```
Jest encountered an unexpected token
...
```

**So then I add ```babel-preset-default``` > ```.babelrc```**
```json
{
    "presets": ["@wordpress/babel-preset-default"]
}
```

**...but then the original error ```Cannot find module '@testing-library/react' from 'src/__test__/edit.test.js'``` comes back**