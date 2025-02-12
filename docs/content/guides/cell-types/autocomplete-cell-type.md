---
title: Autocomplete cell type
metaTitle: Autocomplete cell type - JavaScript Data Grid | Handsontable
description: Collect user input with a list of choices, by using the autocomplete cell type.
permalink: /autocomplete-cell-type
canonicalUrl: /autocomplete-cell-type
react:
  metaTitle: Autocomplete cell type - React Data Grid | Handsontable
searchCategory: Guides
---

# Autocomplete cell type

Collect user input with a list of choices, by using the autocomplete cell type.

[[toc]]

## Overview

Autocomplete can be configured in three different ways:
- Flexible mode
- Strict mode
- Strict mode using Ajax

## Autocomplete flexible mode

This example uses the `autocomplete` feature in the default **flexible mode**. In this mode, the user can choose one of the suggested options while typing or enter a custom value that is not included in the suggestions.

::: only-for javascript
::: example #example1
```js
import Handsontable from 'handsontable';
import 'handsontable/dist/handsontable.full.min.css';

const colors = ['yellow', 'red', 'orange and another color', 'green',
  'blue', 'gray', 'black', 'white', 'purple', 'lime', 'olive', 'cyan'];

const container = document.querySelector('#example1');
const hot = new Handsontable(container, {
  licenseKey: 'non-commercial-and-evaluation',
  data: [
    ['BMW', 2017, 'black', 'black'],
    ['Nissan', 2018, 'blue', 'blue'],
    ['Chrysler', 2019, 'yellow', 'black'],
    ['Volvo', 2020, 'white', 'gray']
  ],
  colHeaders: ['Car', 'Year', 'Chassis color', 'Bumper color'],
  columns: [
    {
      type: 'autocomplete',
      source: ['BMW', 'Chrysler', 'Nissan', 'Suzuki', 'Toyota', 'Volvo'],
      strict: false
    },
    { type: 'numeric' },
    {
      type: 'autocomplete',
      source: colors,
      strict: false,
      visibleRows: 4
    },
    {
      type: 'autocomplete',
      source: colors,
      strict: false,
      trimDropdown: false
    }
  ]
});
```
:::
:::

::: only-for react
::: example #example1 :react
```jsx
import { HotTable } from '@handsontable/react';
import { registerAllModules } from 'handsontable/registry';
import 'handsontable/dist/handsontable.full.min.css';

// register Handsontable's modules
registerAllModules();

export const ExampleComponent = () => {
  const colors = ['yellow', 'red', 'orange and another color', 'green',
    'blue', 'gray', 'black', 'white', 'purple', 'lime', 'olive', 'cyan'
  ];

  return (
    <HotTable
      licenseKey="non-commercial-and-evaluation"
      data={[
        ['BMW', 2017, 'black', 'black'],
        ['Nissan', 2018, 'blue', 'blue'],
        ['Chrysler', 2019, 'yellow', 'black'],
        ['Volvo', 2020, 'white', 'gray']
      ]}
      colHeaders={['Car', 'Year', 'Chassis color', 'Bumper color']}
      columns={[{
          type: 'autocomplete',
          source: ['BMW', 'Chrysler', 'Nissan', 'Suzuki', 'Toyota', 'Volvo'],
          strict: false
        },
        { type: 'numeric' },
        {
          type: 'autocomplete',
          source: colors,
          strict: false,
          visibleRows: 4
        },
        {
          type: 'autocomplete',
          source: colors,
          strict: false,
          trimDropdown: false
        }
      ]}
    />
  );
};

/* start:skip-in-preview */
ReactDOM.render(<ExampleComponent />, document.getElementById('example1'));
/* end:skip-in-preview */
```
:::
:::


## Autocomplete strict mode

This is the same example as above, the difference being that `autocomplete` now runs in **strict mode**. In this mode, the autocomplete cells will only accept values that are defined in the source array. The mouse and keyboard bindings are identical to the `Handsontable` cell type but with the differences below:

* If there is at least one option visible, there always is a selection in HOT-in-HOT
* When the first row is selected, pressing <kbd>**Arrow Up**</kbd> does not deselect HOT-in-HOT. Instead, it behaves as the <kbd>**Enter**</kbd> key but moves the selection in the main HOT upwards

In strict mode, the **allowInvalid** option determines the behaviour in the case of manual user input:

* [`allowInvalid: true`](@/api/options.md#allowinvalid) optional - allows manual input of a value that does not exist in the `source`, the field background is highlighted in red, and the selection advances to the next cell
* [`allowInvalid: false`](@/api/options.md#allowinvalid) - does not allow manual input of a value that does not exist in the `source`, the <kbd>**Enter**</kbd> key is ignored, and the editor field remains open

::: only-for javascript
::: example #example2
```js
import Handsontable from 'handsontable';
import 'handsontable/dist/handsontable.full.min.css';

const colors = ['yellow', 'red', 'orange', 'green', 'blue',
  'gray', 'black', 'white', 'purple', 'lime', 'olive', 'cyan'];
const cars = ['BMW', 'Chrysler', 'Nissan', 'Suzuki', 'Toyota', 'Volvo'];

const container = document.querySelector('#example2');
const hot = new Handsontable(container, {
  licenseKey: 'non-commercial-and-evaluation',
  data: [
    ['BMW', 2017, 'black', 'black'],
    ['Nissan', 2018, 'blue', 'blue'],
    ['Chrysler', 2019, 'yellow', 'black'],
    ['Volvo', 2020, 'white', 'gray']
  ],
  colHeaders: ['Car<br>(allowInvalid true)', 'Year',
    'Chassis color<br>(allowInvalid false)', 'Bumper color<br>(allowInvalid true)'],
  columns: [
    {
      type: 'autocomplete',
      source: cars,
      strict: true
      // allowInvalid: true // true is default
    },
    {},
    {
      type: 'autocomplete',
      source: colors,
      strict: true,
      allowInvalid: false
    },
    {
      type: 'autocomplete',
      source: colors,
      strict: true,
      allowInvalid: true //true is default
    }
  ]
});
```
:::
:::

::: only-for react
::: example #example2 :react
```jsx
import { HotTable } from '@handsontable/react';
import { registerAllModules } from 'handsontable/registry';
import 'handsontable/dist/handsontable.full.min.css';

// register Handsontable's modules
registerAllModules();

export const ExampleComponent = () => {
  const colors = ['yellow', 'red', 'orange', 'green', 'blue',
    'gray', 'black', 'white', 'purple', 'lime', 'olive', 'cyan'
  ];
  const cars = ['BMW', 'Chrysler', 'Nissan', 'Suzuki', 'Toyota', 'Volvo'];

  return (
    <HotTable
      licenseKey="non-commercial-and-evaluation"
      data={[
        ['BMW', 2017, 'black', 'black'],
        ['Nissan', 2018, 'blue', 'blue'],
        ['Chrysler', 2019, 'yellow', 'black'],
        ['Volvo', 2020, 'white', 'gray']
      ]}
      colHeaders={['Car<br>(allowInvalid true)', 'Year', 'Chassis color<br>(allowInvalid false)',
        'Bumper color<br>(allowInvalid true)']}
      columns={[{
          type: 'autocomplete',
          source: cars,
          strict: true
          // allowInvalid: true // true is default
        },
        {},
        {
          type: 'autocomplete',
          source: colors,
          strict: true,
          allowInvalid: false
        },
        {
          type: 'autocomplete',
          source: colors,
          strict: true,
          allowInvalid: true //true is default
        }
      ]}
    />
  );
};

/* start:skip-in-preview */
ReactDOM.render(<ExampleComponent />, document.getElementById('example2'));
/* end:skip-in-preview */
```
:::
:::


## Autocomplete strict mode (Ajax)

Autocomplete can also be used with Ajax data sources. In the example below, suggestions for the "Car" column are loaded from the server. To load data from a remote *asynchronous* source, assign a function to the 'source' property. The function should perform the server-side request and call the callback function when the result is available.

::: only-for javascript
::: example #example3
```js
import Handsontable from 'handsontable';
import 'handsontable/dist/handsontable.full.min.css';

const container = document.querySelector('#example3');
const hot = new Handsontable(container, {
  licenseKey: 'non-commercial-and-evaluation',
  data: [
    ['BMW', 2017, 'black', 'black'],
    ['Nissan', 2018, 'blue', 'blue'],
    ['Chrysler', 2019, 'yellow', 'black'],
    ['Volvo', 2020, 'white', 'gray']
  ],
  colHeaders: ['Car', 'Year', 'Chassis color', 'Bumper color'],
  licenseKey: 'non-commercial-and-evaluation',
  columns: [
    {
      type: 'autocomplete',
      source(query, process) {
        fetch('{{$basePath}}/scripts/json/autocomplete.json')
          .then(response => response.json())
          .then(response => process(response.data));
      },
      strict: true
    },
    {}, // Year is a default text column
    {}, // Chassis color is a default text column
    {} // Bumper color is a default text column
  ]
});
```
:::
:::

::: only-for react
::: example #example3 :react
```jsx
import { HotTable } from '@handsontable/react';
import { registerAllModules } from 'handsontable/registry';
import 'handsontable/dist/handsontable.full.min.css';

// register Handsontable's modules
registerAllModules();

export const ExampleComponent = () => {
  return (
    <HotTable
      licenseKey="non-commercial-and-evaluation"
      data={[
        ['BMW', 2017, 'black', 'black'],
        ['Nissan', 2018, 'blue', 'blue'],
        ['Chrysler', 2019, 'yellow', 'black'],
        ['Volvo', 2020, 'white', 'gray']
      ]}
      colHeaders={['Car', 'Year', 'Chassis color', 'Bumper color']}
      columns={[{
          type: 'autocomplete',
          source(query, process) {
            fetch('{{$basePath}}/scripts/json/autocomplete.json')
                    .then(response => response.json())
                    .then(response => process(response.data));
          },
          strict: true
        },
        {}, // Year is a default text column
        {}, // Chassis color is a default text column
        {} // Bumper color is a default text column
      ]}
    />
  );
};

/* start:skip-in-preview */
ReactDOM.render(<ExampleComponent />, document.getElementById('example3'));
/* end:skip-in-preview */
```
:::
:::


## Related articles

### Related guides

- [Cell type](@/guides/cell-types/cell-type.md)
- [Dropdown cell type](@/guides/cell-types/dropdown-cell-type.md)
- [Select cell type](@/guides/cell-types/select-cell-type.md)

### Related API reference

- Configuration options:
  - [`allowHtml`](@/api/options.md#allowhtml)
  - [`filteringCaseSensitive`](@/api/options.md#filteringcasesensitive)
  - [`sortByRelevance`](@/api/options.md#sortbyrelevance)
  - [`source`](@/api/options.md#source)
  - [`strict`](@/api/options.md#strict)
  - [`trimDropdown`](@/api/options.md#trimdropdown)
  - [`type`](@/api/options.md#type)
  - [`visibleRows`](@/api/options.md#visiblerows)
- Core methods:
  - [`getCellMeta()`](@/api/core.md#getcellmeta)
  - [`getCellMetaAtRow()`](@/api/core.md#getcellmetaatrow)
  - [`getCellsMeta()`](@/api/core.md#getcellsmeta)
  - [`getDataType()`](@/api/core.md#getdatatype)
  - [`setCellMeta()`](@/api/core.md#setcellmeta)
  - [`setCellMetaObject()`](@/api/core.md#setcellmetaobject)
  - [`removeCellMeta()`](@/api/core.md#removecellmeta)
- Hooks:
  - [`afterGetCellMeta`](@/api/hooks.md#aftergetcellmeta)
  - [`afterSetCellMeta`](@/api/hooks.md#aftersetcellmeta)
  - [`beforeGetCellMeta`](@/api/hooks.md#beforegetcellmeta)
  - [`beforeSetCellMeta`](@/api/hooks.md#beforesetcellmeta)
