<script>
    export let selected = false
    export let open = false
    export let href = false
    export let title
    export let icon 
    export let size 

    let iconSize
    $: iconSize = size === "S" ? "ri-sm" : ( size === "M") ? "ri-1x" : ( size === "L" ) ? "ri-lg" : "ri-2x" 
  </script>
  
  <li
    class:is-selected={selected}
    class:is-open={open}
    class="spectrum-TreeView-item"
  >
    <a on:click={() => open = !open} class="spectrum-TreeView-itemLink" {href}>
      {#if $$slots.default}
        <svg
          class="spectrum-Icon spectrum-UIIcon-ChevronRight100 spectrum-TreeView-itemIndicator"
          focusable="false"
          aria-hidden="true"
        >
          <use xlink:href="#spectrum-css-icon-Chevron100" />
        </svg>
      {/if}
      {#if icon}
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <i
          class="{icon} {iconSize}"
        />
      {/if}
      <span class="spectrum-TreeView-itemLabel">{title}</span>
    </a>
    {#if $$slots.default}
      <ul class="spectrum-TreeView spectrum-Treeview--size{size}">
        <slot />
      </ul>
    {/if}
  </li>

  <style>
    i {
      margin-right: 0.25em;
    }
  </style>