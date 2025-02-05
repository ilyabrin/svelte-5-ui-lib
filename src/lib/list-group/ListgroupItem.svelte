<script lang="ts">
  import { getContext } from 'svelte';
  import { twMerge } from 'tailwind-merge';
  import { tv } from 'tailwind-variants';
  import { type ListgroupItemProps as Props, listGroupItem } from '.';

  let { children, onclick, active, current, disabled, name, Icon, href, currentClass = 'text-white bg-primary-700 dark:text-white dark:bg-gray-800', normalClass, disabledClass = 'text-gray-900 bg-gray-100 dark:bg-gray-600 dark:text-gray-400', liClass = 'py-2 px-4 w-full text-sm font-medium list-none first:rounded-t-lg last:rounded-b-lg', class: className, ...restProps }: Props = $props();

  active = getContext('active');

  const itemClass = listGroupItem({
    state: disabled ? 'disabled' : current ? 'current' : 'normal',
    active,
    class: twMerge(liClass, disabled ? disabledClass : current ? currentClass : normalClass, className)
  });

  const buttonClass = tv({
    base: 'flex items-center text-left',
    extend: listGroupItem
  })({
    state: disabled ? 'disabled' : current ? 'current' : 'normal',
    active,
    class: itemClass
  });

  const linkClass = tv({
    base: 'block',
    extend: listGroupItem
  })({
    state: disabled ? 'disabled' : current ? 'current' : 'normal',
    active,
    class: itemClass
  });
</script>

{#if !active && children}
  <li class={itemClass}>
    {@render children()}
  </li>
{:else if href}
  <a {...restProps} {onclick} {href} class={linkClass} aria-current={current}>
    {name}
  </a>
{:else}
  <button {onclick} class={buttonClass} {disabled} aria-current={current}>
    {#if Icon}
      <Icon class="me-2.5 h-5 w-5" />
    {/if}
    {name}
  </button>
{/if}

<!--
@component
[Go to docs](https://svelte-5-ui-lib.codewithshin.com/)
## Props
@prop children
@prop onclick
@prop active
@prop current
@prop disabled
@prop name
@prop Icon
@prop href
@prop currentClass = 'text-white bg-primary-700 dark:text-white dark:bg-gray-800'
@prop normalClass
@prop disabledClass = 'text-gray-900 bg-gray-100 dark:bg-gray-600 dark:text-gray-400'
@prop liClass = 'py-2 px-4 w-full text-sm font-medium list-none first:rounded-t-lg last:rounded-b-lg'
@prop class: className
@prop ...restProps
-->
