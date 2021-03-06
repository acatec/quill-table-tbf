# QuillJS table

Test lab for creating `TABLE` functionality in QuillJS using Containers.

Code of quill is included in project so we can easily play with it in our tests.

## What the previous developers fixed

* Denied Backspace inside an empty cell
* Added ability to delete a table

## What would be nice to add/fix
* the ability to insert tables. Inserts even from Word 
* ctrl+z is still breaking table

## Usage

see example [demo.js](../master/src/demo.js)

```
// import module
import TableModule from "quill-table-tbf"

// register module
Quill.register("modules/table", TableModule);

// add toolbar controls in Toolbar module options
[
        {
            table: TableModule.tableOptions()
        },
        {
            table: ["append-row", "append-col", "remove-col", "remove-row"]
        }
]

// add keboard bindings in Keyboard options

keyboard: {
            bindings: {
                backspace: {
                    key: "backspace",
                    handler: (range, keycontext) =>
                        TableModule.keyboardHandler("backspace", range, keycontext)
                },
                delete: {
                    key: "delete",
                    handler: (range, keycontext) =>
                        TableModule.keyboardHandler("delete", range, keycontext)
                }
            }
        }
```

### For development
```shell script
yarn install

yarn build
```

## Check is it actual or not:

### Progress so far
* `TABLE`, `TR` and `TD` are containers - it is possible to have multiple block blots in `TD`.
* all tables, rows and cells are identified by random strings and optimize merge only those with the same id.
* It is possible to add tables by defining rows and cols count in grid.
* It is possible to add rows and columns to existing tables (accessible by buttons in toolbar).
* it is possible to copy & paste table from Word. Works ok. Needs to test edge cases.

### Known issues
It is early stage so there is a lot of issues with current state.
Still there are some worth to mention which should be dealt with.

* Lists (number or bullet) in cell upon enter loose list format on previous line but keeps it on actual.
* Delete and backspace behavior on tables should be either disabled or should have some well defined behavior. Now it is pretty easy to destroy table in ugly way.
* Definition of TableTrick is hacked in just to test if adding of rows and cols is easily possible - which is. Should be done differently so quill doesn't throw exception (it continues to work).
* Undo/History breaks badly with cell deletions (disabled backspace could solve this).
* When loading delta of nested container in table cell, nested container loose format.
* Containers need order similar to Inline.order. Otherwise delta is not canonical.
* ...
