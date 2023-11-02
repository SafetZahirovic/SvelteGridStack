# Svelte gridstack

## Intro

This is a wrapper component for [Gridstack.js](https://gridstackjs.com/). The sole aim of this component is to take `Gridstack` options, initialize it and wrap it around a `Svelte` component.

Inside, it will convert the data to the component, and export it's item (`GridStackNode`) and index so it's easier to send the data inside the slotted component

## Usage

```svelte
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

```

## Demo

Run `npm run dev`, go to `localhost:5173` (or the port assigned in Vite), and play around with gridstack.

## More

For more info visit Gridstack.js API site

https://github.com/gridstack/gridstack.js/tree/master/doc
