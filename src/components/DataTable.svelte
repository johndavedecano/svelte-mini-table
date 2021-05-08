<script lang="ts">
  import Sortable from 'sortablejs'

  import { createEventDispatcher, onDestroy, onMount } from 'svelte'

  import type { ColumnOption } from './types'

  export let maxHeight: number = 500
  export let stickyHeaders: boolean = true
  export let columns: ColumnOption[] = []
  export let data: any[] = []

  let draggingEl = null
  let startOffset = 0
  let sortableRow: HTMLTableRowElement
  let sortable: any
  let tbody: HTMLTableSectionElement

  let sortedColumns = columns

  const dispatch = createEventDispatcher()

  const onOrderChanged = ({ oldIndex, newIndex }) => {
    const newColumns = [...columns]

    const oldValue = columns[oldIndex]
    const newValue = columns[newIndex]

    newColumns[newIndex] = oldValue
    newColumns[oldIndex] = newValue

    sortedColumns = columns

    dispatch('ordered', newColumns)
  }

  const initializeSortable = () => {
    sortable = Sortable.create(sortableRow, {
      dragClass: 'orderable',
      handle: '.handle',
      onEnd: onOrderChanged,
    })
  }

  const onSortingClicked = (column: ColumnOption) => {
    return () => {
      dispatch('sorted', column)
    }
  }

  const onMouseDown = (evt: any) => {
    evt.preventDefault()
    draggingEl = evt.target.closest('th')
    startOffset = draggingEl.offsetWidth - evt.pageX
    draggingEl.style.cursor = 'col-resize'
  }

  const onMouseMove = (evt: any) => {
    if (draggingEl) {
      evt.preventDefault()
      for (let i = 0; i < evt.pageX; i++) {
        requestAnimationFrame(() => {
          draggingEl.style.minWidth = startOffset + i + 'px'
          draggingEl.style.maxWidth = startOffset + i + 'px'
        })
      }
    }
  }

  const onMouseUp = () => {
    if (draggingEl) {
      draggingEl.style.cursor = ''
      draggingEl = null
      startOffset = 0
      document.body.style.cursor = ''
    }
  }

  onMount(() => {
    initializeSortable()
  })

  onDestroy(() => {
    if (sortable && sortable.destroy) {
      sortable.destroy()
    }
  })
</script>

<svelte:window on:mouseup={onMouseUp} on:mousemove={onMouseMove} />

<div class="wrapper" style="max-height: {maxHeight}px;">
  <table>
    <thead>
      <tr bind:this={sortableRow}>
        {#each sortedColumns as column}
          {#if column.checkbox}
            <th class="checkbox" class:stickyHeader={stickyHeaders}>
              <input type="checkbox" />
            </th>
          {:else}
            <th
              class:stickyHeader={stickyHeaders}
              class:orderable={column.orderable}
            >
              <div class="handle">{column.label}</div>
              {#if column.resizable}
                <div class="resizer" on:mousedown={onMouseDown} />
              {/if}
              {#if column.sorting}
                <div class="sorter" on:click={onSortingClicked(column)} />
              {/if}
            </th>
          {/if}
        {/each}
      </tr>
    </thead>
    <tbody bind:this={tbody}>
      {#each data as item}
        <tr>
          {#each columns as column}
            {#if column.checkbox}
              <td class="checkbox">
                <input type="checkbox" />
              </td>
            {:else if column.component}
              <td>
                <svelte:component
                  this={column.component}
                  {...column.componentProps}
                  {item}
                />
              </td>
            {:else}
              <td>{item[column.name]}</td>
            {/if}
          {/each}
        </tr>
      {/each}
    </tbody>
  </table>
</div>

<style scoped>
  .wrapper {
    position: relative;
    margin: auto;
    overflow: auto;
    max-height: 100%;
    width: 100%;
    border: solid 1px #ddd;
    font-size: 12px;
  }

  .resizer {
    width: 3px;
    height: 100%;
    position: absolute;
    right: 0;
    top: 0;
    cursor: col-resize;
  }

  table {
    border-collapse: separate;
    border-spacing: 0;
    box-sizing: border-box;
    position: relative;
    table-layout: fixed;
    min-width: 100%;
    word-break: break-all;
  }

  th.stickyHeader {
    position: sticky;
    top: 0;
    z-index: 4;
  }

  th.checkbox {
    text-align: center;
    vertical-align: middle;
    padding: 0px;
  }

  th.checkbox > input[type='checkbox'] {
    margin: 0;
    padding: 0;
  }

  th {
    background-color: #fff;
    border-right: solid 1px #ddd;
    border-bottom: solid 1px #ddd;
    overflow: hidden;
    padding: 8px;
    text-align: left;
    text-overflow: ellipsis;
    white-space: nowrap;
    z-index: 1;
    vertical-align: middle;
    line-height: normal;
    position: relative;
    user-select: none;
    min-width: 87px;
  }

  th:last-child {
    border-right: none;
  }

  td {
    background-color: #fff;
    border-right: solid 1px #ddd;
    max-width: 0;
    min-width: 50px;
    overflow: hidden;
    padding: 8px;
    text-overflow: ellipsis;
    white-space: nowrap;
    z-index: 1;
  }

  td:last-child {
    border-right: none;
  }

  td.checkbox {
    text-align: center;
    vertical-align: middle;
    padding: 0px;
  }

  td.checkbox > input[type='checkbox'] {
    margin: 0;
    padding: 0;
  }

  tbody tr:hover td {
    background-color: #e1f4ff;
  }

  div.sorter {
    position: absolute;
    height: 30px;
    width: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    right: 2px;
    top: 0;
    cursor: pointer;
  }

  div.sorter:before,
  div.sorter:after {
    border: 4px solid transparent;
    content: '';
    display: block;
    height: 0;
    width: 0;
  }

  div.sorter:before {
    border-bottom-color: #666;
    margin-top: -10px;
  }

  div.sorter:after {
    border-top-color: #666;
    margin-top: 8px;
    margin-left: -8px;
  }
</style>
