<script lang="ts">
  import { SvelteGridStack } from "$lib/index.js";
  import type {
    GridItemHTMLElement,
    GridStackOptions,
    GridStackWidget,
  } from "gridstack";

  //GridsStack item configuration
  //https://github.com/gridstack/gridstack.js/tree/master/doc#loadlayout-gridstackwidget-boolean--w-gridstackwidget-add-boolean--void---true

  const items = Array.from({ length: 10 }).fill({ w: 2 }) as GridStackWidget[];

  //Gridstack initial options
  //https://github.com/gridstack/gridstack.js/tree/master/doc#api-global-static
  const opts: GridStackOptions = {
    margin: 10,
  };

  //Handle event from GridStack
  //https://github.com/gridstack/gridstack.js/tree/master/doc#resizeevent-el
  const handleResize = ({
    detail,
  }: {
    detail: {
      event: Event;
      el: GridItemHTMLElement;
    };
  }) => {
    console.log("RESIZED", detail);
  };
</script>

<SvelteGridStack {opts} {items} on:resize={handleResize} let:index let:item>
  <div>
    Props:
    <pre>{JSON.stringify(item)}</pre>
    Index:
    <pre>{index}</pre>
  </div>
</SvelteGridStack>
