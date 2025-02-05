<script lang="ts">
  import { getContext } from 'svelte';
  import { twMerge } from 'tailwind-merge';
  import { page } from '$app/stores';
  import type { navbarType } from '$lib/types';
  import { type NavLiProps as Props } from '.';

  let { closeNav, href, children, aClass, activeClass, nonActiveClass, class: className, ...restProps }: Props = $props();

  let breakPoint: navbarType['breakPoint'];

  const context = getContext<navbarType>('navbarContext');
  breakPoint = context.breakPoint ?? 'md';
  closeNav = context.closeNav ?? closeNav;
  let currentUrl = $state($page.url.pathname);
  $effect(() => {
    currentUrl = $page.url.pathname;
  });
  const linkBreaks = {
    md: 'md:hover:bg-transparent md:border-0 md:hover:text-primary-700 md:p-0 md:dark:hover:text-primary-500 md:dark:hover:bg-transparent',
    lg: 'lg:hover:bg-transparent lg:border-0 lg:hover:text-primary-700 lg:p-0 lg:dark:hover:text-primary-500 lg:dark:hover:bg-transparent',
    xl: 'xl:hover:bg-transparent xl:border-0 xl:hover:text-primary-700 xl:p-0 xl:dark:hover:text-primary-500 xl:dark:hover:bg-transparent',
    xxl: '2xl:hover:bg-transparent 2xl:border-0 2xl:hover:text-primary-700 2xl:p-0 2xl:dark:hover:text-primary-500 2xl:dark:hover:bg-transparent'
  };

  const activeBreaks = {
    md: 'md:bg-transparent md:text-primary-700 md:p-0 md:dark:text-primary-500',
    lg: 'lg:bg-transparent lg:text-primary-700 lg:p-0 lg:dark:text-primary-500',
    xl: 'xl:bg-transparent xl:text-primary-700 xl:p-0 xl:dark:text-primary-500',
    xxl: '2xl:bg-transparent 2xl:text-primary-700 2xl:p-0 2xl:dark:text-primary-500'
  };

  let aCls = $derived(currentUrl === href ? (activeClass ?? twMerge(context.activeClass, activeBreaks[breakPoint])) : (nonActiveClass ?? twMerge(context.nonActiveClass, linkBreaks[breakPoint])));

  let linkClass = $derived(twMerge(aCls, aClass));
</script>

<li class={className}>
  <a {href} onclick={closeNav} {...restProps} aria-current={currentUrl === href} class={linkClass}>
    {@render children()}
  </a>
</li>

<!--
@component
[Go to docs](https://svelte-5-ui-lib.codewithshin.com/)
## Props
@prop closeNav
@prop href
@prop children
@prop aClass
@prop activeClass
@prop nonActiveClass
@prop class: className
@prop ...restProps
-->
