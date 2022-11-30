<script>
  import "@spectrum-css/treeview/dist/index-vars.css"
  import TreeItem from "./TreeItem.svelte";
  import { getContext } from "svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")
  const loading = getContext("loading")

  export let quiet = false
  export let standalone = true
  export let width = "250px"
  export let dataProvider 
  export let showColumn = "_id"
  export let childColumn = ""
  export let icon
  export let size

    // If the parent DataProvider is loading, fill the rows array with a number of empty objects 
    // corresponding to the DataProvider's page size; 
    // this allows skeleton loader components to be rendered further down the tree.
  $: rows = $loading
    ? new Array(dataProvider?.limit > 20 ? 20 : dataProvider?.limit).fill({})
    : dataProvider?.rows
</script>

<div use:styleable={$component.styles}>
    <ul
      class:spectrum-TreeView--standalone={standalone}
      class:spectrum-TreeView--quiet={quiet}
      class="spectrum-TreeView spectrum-Treeview--size{size}"
      style="width: {width}"
    >
    {#if rows?.length > 0}
      {#each rows as row }
        <TreeItem title={row[showColumn]} {icon} {size}>
        {#if row[childColumn] && Array.isArray(row[childColumn])}
          {#each row[childColumn] as child}
            <TreeItem title={child.primaryDisplay} {icon} {size} >  </TreeItem>
          {/each}
        {/if}
        </TreeItem>
      {/each}
      {/if}
      <slot />
  </ul>
</div>