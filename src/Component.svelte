<script>
  import TreeItem from "./TreeItem.svelte";
  import { getContext } from "svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")
  const loading = getContext("loading")

  export let quiet = false
  export let standalone = true
  export let width = "250px"
  export let dataProvider 
  export let parentValue
  export let parentID
  export let parentIcon
  export let parentOnClick
  export let childColumn
  export let childIcon
  export let childOnClick
  export let size

  let linkTable

    // If the parent DataProvider is loading, fill the rows array with a number of empty objects 
    // corresponding to the DataProvider's page size; 
    // this allows skeleton loader components to be rendered further down the tree.
  $: rows = $loading
    ? new Array(dataProvider?.limit > 20 ? 20 : dataProvider?.limit).fill({})
    : dataProvider?.rows

  $: linkTable = dataProvider?.schema[childColumn]?.type === "link"
</script>
<div use:styleable={$component.styles}>
    {#if !linkTable}
      <span class="error"> Child Column not a Relationship </span>
    {/if}

    <ul
      class:spectrum-TreeView--standalone={standalone}
      class:spectrum-TreeView--quiet={quiet}
      class="spectrum-TreeView spectrum-Treeview--size{size}"
      style="width: {width}"
    >
    
    {#if rows?.length > 0}
      {#each rows as row }
        <TreeItem 
          id={row[parentID]} 
          title={row[parentValue]} 
          icon={parentIcon} {size} 
          onClick={parentOnClick}
          >
          {#if row[childColumn] && Array.isArray(row[childColumn])}
            {#each row[childColumn] as child}
              <TreeItem 
                id={child._id} 
                title={child.primaryDisplay}
                onClick={childOnClick}
                icon={childIcon} {size} >
              </TreeItem>
            {/each}
          {/if}
        </TreeItem>
      {/each}
    {/if}
      <slot />
  </ul>
</div>

<style>
  .error {
    border: 2px dotted red;
    margin: 1rem;
    padding: 0.5rem 1rem;
  }
</style>