# Svelte Mini Table

A simple data table implementation on svelte.js.

- Draggable table headers
- Custom renderer
- Sticky table headers
- Resizable columns
- ... and more


![My Image](https://raw.github.com/johndavedecano/svelte-mini-table/master/screenshot.png)

## Installation



## Sample

```
  <DataTable
    {columns}
    {data}
    maxHeight={500}
    on:sorted={onSorted}
    on:ordered={onOrdered}
  />
```

**Columns**

```
  let columns: ColumnOption[] = [
    {
      id: 1,
      label: 'Name',
      name: 'name',
      sorting: true,
      orderable: true,
      resizable: true,
    },
    {
      id: 2,
      label: 'Phone',
      name: 'phone',
      sorting: true,
      orderable: true,
      resizable: true,
    },
    {
      id: 3,
      label: 'City',
      name: 'city',
      sorting: true,
      orderable: true,
      resizable: true,
    },
    {
      id: 4,
      label: 'Province',
      name: 'province',
      sorting: true,
      orderable: true,
      resizable: true,
    },
    {
      id: 5,
      label: 'Region',
      name: 'region',
      sorting: true,
      orderable: true,
      resizable: true,
    },
    {
      id: 6,
      label: 'Country',
      name: 'country',
      sorting: true,
      orderable: true,
      resizable: true,
    },
    {
      id: 7,
      label: 'Action',
      name: 'action',
      resizable: true,
      orderable: true,
      component: Action,
      componentProps: {
        onClick: (item: ColumnOption) => {
          console.log(item)
        },
      },
    },
  ]
```