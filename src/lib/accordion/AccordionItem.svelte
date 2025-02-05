<script lang="ts">
  import { twMerge } from 'tailwind-merge';
  import { slide } from 'svelte/transition';
  import { getContext } from 'svelte';
  import { writable } from 'svelte/store';
  import { type AccordionCtxType, type AccordionItemProps as Props } from '.';
  import type { ParamsType } from '../types';

  let { children, header, arrowup, arrowdown, open = $bindable(false), activeClass = undefined, inactiveClass = undefined, defaultClass = 'flex items-center justify-between w-full font-medium text-left group-first:rounded-t-xl border-gray-200 dark:border-gray-700', transition = slide, params, paddingFlush = 'py-5', paddingDefault = 'p-5', textFlushOpen = 'text-gray-900 dark:text-white', textFlushDefault = 'text-gray-500 dark:text-gray-400', borderClass = 'border-s border-e group-first:border-t', borderOpenClass = 'border-s border-e', borderBottomClass = 'border-b', borderSharedClass = 'border-gray-200 dark:border-gray-700', classActive = undefined, classInactive = undefined, class: className }: Props = $props();

  let activeCls = twMerge(activeClass, classActive);
  let inactiveCls = twMerge(inactiveClass, classInactive);

  const ctx = getContext<AccordionCtxType>('ctx') ?? {};

  // single selection
  const self = {};
  const selected = ctx.selected ?? writable();

  let _open: boolean = $state(open);

  $effect(() => {
    if (_open) $selected = self;

    // this will trigger unsubscribe on destroy
    return selected.subscribe((x) => (open = x === self));
  });

  const handleToggle = (_: Event) => selected.set(open ? {} : self);

  let buttonClass: string = twMerge(defaultClass, ctx.flush ? '' : borderClass, borderBottomClass, borderSharedClass, ctx.flush ? paddingFlush : paddingDefault, open && (ctx.flush ? textFlushOpen : activeCls || ctx.activeClass), !open && (ctx.flush ? textFlushDefault : inactiveCls || ctx.inactiveClass), className);

  let contentClass = twMerge([ctx.flush ? paddingFlush : paddingDefault, ctx.flush ? '' : borderOpenClass, borderBottomClass, borderSharedClass]);
</script>

<h2 class="group">
  <button onclick={handleToggle} type="button" class={buttonClass} aria-expanded={open}>
    {#if header}
      {@render header()}
      {#if open}
        {#if !arrowup}
          <svg class="h-3 w-3 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5 5 1 1 5" />
          </svg>
        {:else}
          {@render arrowup()}
        {/if}
      {:else if !arrowdown}
        <svg class="h-3 w-3 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
          <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4" />
        </svg>
      {:else}
        {@render arrowdown()}
      {/if}
    {/if}
  </button>
</h2>
{#if open}
  <div transition:transition={params as ParamsType}>
    <div class={contentClass}>
      {@render children()}
    </div>
  </div>
{/if}

<!--
@component
[Go to docs](https://svelte-5-ui-lib.codewithshin.com/)
## Props
@prop children
@prop header
@prop arrowup
@prop arrowdown
@prop open = $bindable(false)
@prop activeClass = undefined
@prop inactiveClass = undefined
@prop defaultClass = 'flex items-center justify-between w-full font-medium text-left group-first:rounded-t-xl border-gray-200 dark:border-gray-700'
@prop transition = slide
@prop params
@prop paddingFlush = 'py-5'
@prop paddingDefault = 'p-5'
@prop textFlushOpen = 'text-gray-900 dark:text-white'
@prop textFlushDefault = 'text-gray-500 dark:text-gray-400'
@prop borderClass = 'border-s border-e group-first:border-t'
@prop borderOpenClass = 'border-s border-e'
@prop borderBottomClass = 'border-b'
@prop borderSharedClass = 'border-gray-200 dark:border-gray-700'
@prop classActive = undefined
@prop classInactive = undefined
@prop class: className
-->
