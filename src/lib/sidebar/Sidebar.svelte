<script lang="ts">
  import type { Snippet } from 'svelte';
  import { setContext } from 'svelte';
  import type { SidebarType } from '$lib/types';
  import { twMerge } from 'tailwind-merge';
  import type { HTMLAttributes } from 'svelte/elements';

  interface Props extends HTMLAttributes<HTMLElement> {
    children: Snippet;
    asideClass?: string;
    ariaLabel?: string;
    divClass?: string;
    nonActiveClass?: string;
    activeClass?: string;
  }
  let { children, divClass, asideClass, ariaLabel, nonActiveClass = '', activeClass = '', class: className, ...restProps }: Props = $props();

  const activeCls = 'flex items-center text-base font-normal text-gray-900 bg-gray-200 dark:bg-gray-700 rounded dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700';
  const nonActiveCls = 'flex items-center text-base font-normal text-gray-900 rounded dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700';
  let divCls = twMerge('overflow-y-auto py-4 px-3 bg-gray-50 rounded dark:bg-gray-800', divClass);

  setContext<SidebarType>('sidebarContext', {
    activeClass: twMerge(activeCls, activeClass, className),
    nonActiveClass: twMerge(nonActiveCls, nonActiveClass, className)
  });
  // $inspect('activeClass: ', activeClass)
</script>

<aside {...restProps} class={twMerge('w-64', asideClass)} aria-label={ariaLabel}>
  <div class={twMerge(divCls)}>
    {@render children()}
  </div>
</aside>

<!--
@component
[Go to docs](https://svelte-5-ui-lib.codewithshin.com/)
## Props
@prop children
@prop divClass
@prop asideClass
@prop ariaLabel
@prop nonActiveClass = ''
@prop activeClass = ''
@prop class: className
@prop ...restProps
-->
