<script>
  export let item = {}

  export let onClick = () => {}
</script>

<div on:click={() => onClick(item)}>{item.name}</div>
