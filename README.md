# Excel Editor for VUE 2

Vue2 plugin for displaying and editing the array-of-object in Excel style. It supports the following features:

- Excel-like UI
- Real 2-way data binding
- Column Filtering
- Column Sorting
- Pagination
- Row selection
- Update the cells in all selected rows
- Key support: up, down, left, right, page-up, page-down, tab, esc
- Ctrl/meta Key support: Ctrl-A, Ctrl-C, Ctrl-V, Ctrl-Z, Ctrl-F, Ctrl-G, Ctrl-L
- Column Valiation
- Cell Error Tooltip
- Custom Column Header
- Readonly Column
- Column Visibility
- Column Sequence
- Column Width Adjustment
- Undo
- Copy & Paste
- Mass Import Excel Data

## Getting started

Get the package:
```bash
npm install vue-excel-editor
```

Register VueExcelEditor in your app entrypoint:
```js
import Vue from 'vue'
import VueExcelEditor from 'vue-excel-editor'

Vue.use(VueExcelEditor)
```

In your template
```html
<template>
    <vue-excel-editor v-model="jsondata">
        <vue-excel-column field="user"   label="User" />
        <vue-excel-column field="name"   label="Name" />
        <vue-excel-column field="phone"  label="Contact" />
        <vue-excel-column field="gender" label="Gender" />
        <vue-excel-column field="age"    label="Age" />
        <vue-excel-column field="birth"  label="Date Of Birth" />
    </vue-excel-editor>
</template>

```

## Outlook
![Simple Outlook](https://i.ibb.co/F6W7Xpg/simple.png "VueExcelEditor")

## Props List

#### Component: vue-excel-editor
| Name            | Mandatory | Type     | Description |
| :---            | :---      | :---     | :---        |
| v-model         | Mandatory | Array    | Edited data in Array Of Object | 
| page            | Optional  | Number   | Specific page size, default is auto-calculating by screen height |
| no-paging       | Optional  | Boolean  | Disable paging feature, default is false |
| no-num-col      | Optional  | Boolean  | No number column, default is false |
| filter-row      | Optional  | Boolean  | Show fixed filter row, default is false |
| no-footer       | Optional  | Boolean  | No footer row, default is false |
| no-finding      | Optional  | Boolean  | Disable find key (ctrl-f) and finding dialog, default is false |
| no-finding-next | Optional  | Boolean  | Disable find-next key (ctrl-g), default is false |
| free-select     | Optional  | Boolean  | Select multiple rows without pressing ctrl/meta key |
| autocomplete    | Optional  | Boolean  | Enable autocomplete of all columns, default is false |
| readonly        | Optional  | Boolean  | Set all columns read only, default is false |
| readonly-style  | Optional  | Object   | The style of the read-only cell |
| height          | Optional  | String   | Define the exact height in px of the component, default is 'auto' |
| width           | Optional  | String   | Define the maximum width in px of the component, default is '100%' |
| row-style       | Optional  | Function | Conditional row formatting, default is null |
| cell-style      | Optional  | Function | Conditional cell formatting, default is null |
| header-label    | Optional  | Function | Func to return the label, parameter are the field label, field object |
| record-label    | Optional  | Function | Func to return the label, parameters are recordPosition, record object |
| localized-label | Optional  | Object   | Customize labels and messages for localization purpose |
| n-filter-count  | Optional  | Number   | Number of items to be listed in filter dialog. Default is 200 |
| remember        | Optional  | Boolean  | Remember the setting in localStorage, default is false |
| autoAddRow        | Optional  | Function  | 自动调用新增行，(注意:在po中因为判断productId不为空，应该在赋值productId后手动调用一次) |
| sortable        | Optional  | Boolean  | 全局禁用sort | 

#### Component: vue-excel-column
| Name           | Mandatory | Type     | Description |
| :---           | :---      | :---     | :---        |
| field          | Mandatory | String   | Field name, row object key |
| label          | Optional  | String   | Header label, default is field name |
| type           | Optional  | String   | Column type: 'string'(default), 'number', 'select', 'check10', 'checkYN', 'checkTF', 'date', 'datetime', 'datetimesec', 'datetick', 'datetimetick', 'datetimesectick','custom'  注意custom类型需要配合@customclick一起使用|
| readonly       | Optional  | Boolean|Function  | 支持bool或方法判断(row)=>{ return true} |
| init-style     | Optional  | Object   | Cell inital style in css |
| sticky         | Optional  | Boolean  | Fixed column at left of the table, no response on horizontal scrolling |
| invisible      | Optional  | Boolean  | Column visibility, default is false |
| width          | Optional  | String   | Specified column width, default is '100px' |
| change         | Optional  | Function | Custom function to be triggered when the data of this column changed |
| validate       | Optional  | Function | Custom function to validate and return the error message |
| key-field      | Optional  | Boolean  | Specified the key field which is included in keys parameter in @update event |
| allow-keys     | Optional  | Array    | Array of char which allow to input |
| mandatory      | Optional  | String   | If specified, it is showed as error when it modified as blank, default is '' |
| length-limit   | Optional  | Number   | Not allow to input when the content length reaches the limit |
| autocomplete   | Optional  | Boolean  | Allow autocomplete popup when editing, default is parent prop: autocomplete |
| pos            | Optional  | Number   | Specified column sequence |
| text-transform | Optional  | String   | Force the input to upppercase or lowercase when editing |
| text-align     | Optional  | String   | Text alignment, default is 'left' |
| options        | Optional  | Array    | Define the selectable options, if type != 'select, it works as autocomplete 扩展了回调方法 row=>{ return row.xxList } |
| selectOptions        | Optional  | Object    | 如果使用了回调options，返回对象； 则使用selectOptions设置label,values属性名 以及回调方法 {label:.. value:... setValue:(selectItem, rowData)=>{ rowData.xx=selectItem.value }} |
| summary        | Optional  | String|Function   | Summary: 'sum', 'avg', 'max', 'min'. Default is null；扩展了方法回调()=>{return 999} |
| to-text        | Optional  | Function | The custom conversion function from object value to edit-text 例 (v)=>format(v) |
| to-value       | Optional  | Function | The custom conversion function from edit-text to object value 例:(v)=>moment(v) |

## Hot Key List

| Name        | Condition    | Description |
| :---        | :---         | :---        |
| Ctrl/Meta A | Table Focus  | Select all rows |
| Ctrl/Meta C | Cell Focus   | Select the cell text to clipboard |
| Ctrl/Meta V | Cell Focus   | Place the clipboard text to cell |
| Ctrl/Meta Z | Table Focus  | Undo the last update |
| Ctrl/Meta F | Table Focus  | Popup the "Find" dialog |
| Ctrl/Meta G | After "Find" | Continue to find the text |
| Ctrl/Meta L | Cell Focus   | Force to show autocomplete list, or the option list for "select" typed column |

## Events List

#### Component: vue-excel-editor
| Name    | Argument                  | Description |
| :---    | :---                      | :---        |
| update  | Array Of Array            | Update Cell information |
| select  | Array of rows, select/not | Emit when rows are selected/unselected |
| setting | setting                   | Emit when setting (column width, invisible state) is changed |
| rowChange  | 行改变（注意：新绑定行时需要手动指定，默认第一行） |
| removeRows  | rows=>{ ... } 批量删除行  |

#### Component: vue-excel-column
| Name    | Argument                  | Description |
| :---    | :---                      | :---        |
| customclick  | {row,cellWriter}           | arg=>{ arg.cellWriter("需要写入的字符") }  该事件用于弹出窗口选择后回给单元格赋值 |


(TBD)

## Methods List

#### Component: vue-excel-editor
| Name               | Arguments                     | Description |
| :---               | :---                          | :---        |
| firstPage          |                               | Move to the first page |
| lastPage           |                               | Move to the last page |
| prevPage           |                               | Move to the previous page |
| nextPage           |                               | Move to the next page |
| moveNorth          |                               | Move the cursor cell to upper cell |
| moveSouth          |                               | Move the cursor cell to lower cell |
| moveWest           |                               | Move the cursor cell to previous cell |
| moveEast           |                               | Move the cursor cell to next cell |
| doFind             | text                          | Find the specified text in whole table and locate the cursor cell |
| doFindNext         |                               | Contnue the last find |
| sort               | n, pos                        | Sort the column specified by pos, n = 1 (ascending) or -1 (descending) |
| selectRecord       | row                           | Select the row |
| unSelectRecord     | row                           | UnSelect the row |
| clearAllSelected   |                               | Unselect all selected rows |
| getSelectedRecords |                               | Get an array of the selected rows |
| exportTable        | format*, selectedOnly*, name* | Export the filtered table as xlsx/csv |
| importTable        | callback*                     | Import the specified formatted xlsx |
| undoTransaction    |                               | Undo the last update |
| setFilter          | name, text                    | Set the filter text on column name |
| clearFilter        | name*                         | Clear the filter text on column name |
| columnSuppress     |                               | Hide the column if all values are null or empty |

* = optional

## Variable List

#### Component: vue-excel-editor
| Name          | Type            | Description |
| :---          | :---            | :---        |
| fields        | Array of Object | It contains the column spec, each will create during mount, developer can still change the col spec via this after mounted |
| filterColumn  | Object          | Contains the current filters, developer can access the filter string via this |
| selected      | Object          | Contains all the selected rows, the key is row number and the value is internal $id |
| selectedCount | Number          | Number of rows are selected |
| errmsg        | Object          | Contains all the validation error messages, the key is internal $id plus field name |
| redo          | Array of array  | The buffer of undo, it will be removed after undo or table changed |
| pageTop       | Number          | The top row number of the current page |

## Example

```html
<template>
    <vue-excel-editor v-model="jsondata" filter-row>
        <vue-excel-column field="user"   label="User ID"       type="string" width="80px" />
        <vue-excel-column field="name"   label="Name"          type="string" width="150px" />
        <vue-excel-column field="phone"  label="Contact"       type="string" width="130px" />
        <vue-excel-column field="gender" label="Gender"        type="select" width="50px" :options="['F','M','U']" />
        <vue-excel-column field="age"    label="Age"           type="number" width="70px" />
        <vue-excel-column field="birth"  label="Date Of Birth" type="date"   width="80px" />
    </vue-excel-editor>
</template>
```
```js
export default {
    name: 'app',
    data: {
        jsondata: [
            {user: 'kc', name: 'Kenneth Cheng', phone: '852-1234-5678', gender: 'M', age: 25, birth: '1997-07-01'},
            {user: 'sm', name: 'Simon Minolta', phone: '852-1234-5682', gender: 'M', age: 20, birth: '1999-11-12'},
            {user: 'ra', name: 'Raymond Atom',  phone: '852-1234-5683', gender: 'M', age: 18, birth: '2000-06-11'},
            {user: 'ag', name: 'Mary George',   phone: '852-1234-5684', gender: 'F', age: 22, birth: '2002-08-01'},
            {user: 'kl', name: 'Kenny Linus',   phone: '852-1234-5685', gender: 'M', age: 29, birth: '1990-09-01'}
        ]
    }
}
```

### Work with redis for saving

```html
<vue-excel-editor v-model="jsondata" @update="onSave">
...
</vue-excel-editor>
```
```js
methods: {
    onSave (records) {
      records = records.map(rec => ['hset', rec.keys.join(), rec.name, rec.newVal])
      redis.multi(records).exec()
    }
}
```

### Remember the grid setting
The grid setting such as column width can be saved in the localStorage of client browser
```html
<template>
    <vue-excel-editor v-model="jsondata" remember>
    ...
    </vue-excel-editor>
</template>
```

### Do something when user select the rows
The selected rows will be passed to the provided trigger method
```html
<template>
    <vue-excel-editor v-model="jsondata" @select="onSelect">
    ...
    </vue-excel-editor>
</template>
```
```js
methods: {
    onSelect (selectedRows) {
      console.log(selectedRows)
    }
}
```

### Other Features

```html
<template>
    <vue-excel-editor v-model="jsondata" no-paging autocomplete filter-row>
        <vue-excel-column field="user"   label="User ID"       type="string" width="80px" readonly key-field sticky />
        <vue-excel-column field="name"   label="Name"          type="string" width="150px" />
        <vue-excel-column field="phone"  label="Contact"       type="string" width="130px" :validate="validPhoneNum" />
        <vue-excel-column field="gender" label="Gender"        type="select" width="50px" :options="['F','M','U']" />
        <vue-excel-column field="age"    label="Age"           type="number" width="70px" />
        <vue-excel-column field="birth"  label="Date Of Birth" type="date"   width="80px" />
    </vue-excel-editor>
</template>
```

#### Filter + Footer Rows
![Filter + Footer Rows](https://i.ibb.co/0CtB6Hg/footerfilter.png "Filter + Footer Rows")

#### Filtering
![Filtering](https://i.ibb.co/VDB7jDd/filtering.png "Filtering")

#### Sorting
![Sorting](https://i.ibb.co/ggtJxYm/sorting.png "Sorting")

#### Options
![Options](https://i.ibb.co/GQ6rKBM/options.png "Options")

#### Autocomplete
![Autocomplete](https://i.ibb.co/7VRtFhT/autocomplete.png "Autocomplete")

#### Select
![Select](https://i.ibb.co/GxJGtrv/select.png "Select")

#### Validation
```html
<vue-excel-column field="phone" label="Contact" type="string" width="130px" :validate="validPhoneNum" />
```
```js
methods: {
    validPhoneNum (content) {
        if (content === '') return 'Mandatory field'
        if (!/^[0-9]{3}-[0-9]{4}-[0-9]{4}$/.test(content)) return 'Invalid Phone Number'
        return ''
    }
}
```

![Validation](https://i.ibb.co/tzbKmMt/validate.png "Validation")

#### Summary
```html
<vue-excel-column field="age"   label="Age"           type="number" width="70px" summary="sum" />
<vue-excel-column field="birth" label="Date Of Birth" type="date"   width="80px" summary="min" />
```

![Summary](https://i.ibb.co/0B26s9Q/summary.png "Summary")

"sum", "min", "max", "avg" are supported.

## Localization
Developer may override the default values through localized-label prop
```html
<template>
    <vue-excel-editor v-model="jsondata" :localized-label="myLabels">
    ...
    </vue-excel-editor>
</template>
```

```javascript
data: {
    myLabels = {
        footerLeft: (top, bottom, total) => `Record ${top} to ${bottom} of ${total}`,
        first: 'First',
        previous: 'Previous',
        next: 'Next',
        last: 'Last',
        footerRight: {
            selected: 'Selected:',
            filtered: 'Filtered:',
            loaded: 'Loaded:'
        },
        processing: 'Processing',
        tableSetting: 'Table Setting',
        exportExcel: 'Export Excel',
        importExcel: 'Import Excel',
        back: 'Back',
        reset: 'Default',
        sortingAndFiltering: 'Sorting And Filtering',
        sortAscending: 'Sort Ascending',
        sortDescending: 'Sort Descending',
        near: '≒ Near',
        exactMatch: '= Exact Match',
        notMatch: '≠ Not Match',
        greaterThan: '&gt; Greater Than',
        greaterThanOrEqualTo: '≥ Greater Than or Equal To',
        lessThan: '&lt; Less Than',
        lessThanOrEqualTo: '≤ Less Than Or Equal To',
        regularExpression: '~ Regular Expression',
        customFilter: 'Custom Filter',
        listFirstNValuesOnly: n => `List first ${n} values only`,
        apply: 'Apply',
        noRecordIsRead: 'No record is read',
        readonlyColumnDetected: 'Readonly column detected',
        columnHasValidationError: (name, err) => `Column ${name} has validation error: ${err}`,
        noMatchedColumnName: 'No matched column name',
        invalidInputValue: 'Invalid input value',
        missingKeyColumn: 'Missing key column'
    }
}
```

## Compatibility
Chrome 79+, FireFox 71+, Safari 13+

## License
MIT

## Status
This project is in an early stage of development. Any contribution is welcome :D
