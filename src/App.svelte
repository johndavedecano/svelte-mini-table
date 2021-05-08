<script lang="ts">
  import DataTable from './components/DataTable.svelte'

  import type { ColumnOption } from './components/types'

  import Action from './Action.svelte'

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

  let data = Array.from(new Array(1000)).map((_, o) => {
    return {
      id: o + 1,
      name: 'Sample User ' + o,
      phone: '0290423423423' + o,
      city: 'Pasig',
      province: 'Metro Manila',
      region: 'NCR',
      country: 'PH',
    }
  })

  const onSorted = ({ detail }) => {
    console.log(detail)
  }

  const onOrdered = ({ detail }) => {
    columns = detail
  }
</script>

<div class="container">
  <DataTable
    {columns}
    {data}
    maxHeight={500}
    on:sorted={onSorted}
    on:ordered={onOrdered}
  />
</div>

<style>
  .container {
    width: 1000px;
    margin: auto;
    font-size: 14px;
  }
</style>
