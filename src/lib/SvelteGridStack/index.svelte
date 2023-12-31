<script lang="ts">
  import type {
    GridStack,
    GridStackNode,
    GridStackOptions,
    GridStackWidget,
  } from "gridstack";
  import { createEventDispatcher, onDestroy, onMount } from "svelte";

  import "gridstack/dist/gridstack-extra.min.css";
  import "gridstack/dist/gridstack.min.css";
  import type {
    GridstackCallbackParams,
    GridstackDispatchEvents,
  } from "./types.js";

  export let items: Array<GridStackWidget>;
  export let opts: GridStackOptions;

  const gridStackEvents = [
    "added",
    "change",
    "disable",
    "drag",
    "dragstart",
    "dragstop",
    "dropped",
    "enable",
    "removed",
    "resize",
    "resizestart",
    "resizestop",
    "resizecontent",
  ] as const;

  const dispatch = createEventDispatcher<GridstackDispatchEvents>();

  let gridEl: HTMLDivElement;
  let grid: GridStack;

  onMount(async () => {
    const { GridStack } = await import("GridStack");
    grid = GridStack.init(opts);
    grid.on("added", (_: Event, items: Array<GridStackNode>) => {
      items.forEach((item, index) => {
        const element = gridEl.querySelector(
          `#grid-id-${index}`
        ) as HTMLDivElement;
        const child = item.el?.firstElementChild;

        if (!child || !element) {
          console.error("Cannot append append element to GridStack");
          return;
        }
        child.appendChild(element);
        element.style.display = "block";
      });
    });

    gridStackEvents.forEach((ev) => {
      grid.on(ev, (args: GridstackCallbackParams) => {
        dispatch(ev, args);
      });
    });

    grid.load(items);
  });

  onDestroy(() => {
    gridStackEvents.forEach((event) => grid?.off(event));
  });
</script>

<main>
  <div bind:this={gridEl} class="grid-stack">
    {#each items as item, index}
      <div style="display:none" id={`grid-id-${index}`}>
        <slot {index} {item} />
      </div>
    {/each}
  </div>
</main>

<style>
  .grid-stack {
    background: #fafad2;
  }
  :global(.grid-stack-item-content) {
    background-color: #18bc9c;
  }
</style>
