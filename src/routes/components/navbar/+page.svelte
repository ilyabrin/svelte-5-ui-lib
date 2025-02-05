<script lang="ts">
  import { Navbar, NavLi, NavBrand, NavUl, uiHelpers, ImagePlaceholder, Skeleton, TextPlaceholder, P } from '$lib';
  import HighlightCompo from '../../utils/HighlightCompo.svelte';
  import CodeWrapper from '../../utils/CodeWrapper.svelte';
  import H1 from '../../utils/H1.svelte';
  import H2 from '../../utils/H2.svelte';
  // for Props table
  import CompoAttributesViewer from '../../utils/CompoAttributesViewer.svelte';
  const dirName = 'nav';
  const modules = import.meta.glob('./md/*.md', {
    query: '?raw',
    import: 'default',
    eager: true
  });

  let breakPoint: Navbar['breakpoint'] = $state('md');
  let nav = uiHelpers();
  let navStatus = $state(false);
  let toggleNav = nav.toggle;
  let closeNav = nav.close;

  let nav2 = uiHelpers();
  let navStatus2 = $state(false);
  let toggleNav2 = nav2.toggle;
  let closeNav2 = nav2.close;

  let navLg = uiHelpers();
  let navStatusLg = $state(false);
  let toggleNavLg = navLg.toggle;
  let closeNavLg = navLg.close;

  $effect(() => {
    // this can be done adding nav.navStatus directly to DOM element
    // without using effect
    navStatus = nav.isOpen;
    navStatus2 = nav2.isOpen;
    navStatusLg = navLg.isOpen;
  });
</script>

<H1>Navbar</H1>

<H2>Default Nav</H2>

<CodeWrapper>
  <Navbar {toggleNav} {closeNav} {navStatus} {breakPoint}>
    {#snippet brand()}
      <NavBrand siteName="Svelte 5">
        <img width="30" src="/images/svelte-icon.png" alt="svelte icon" />
      </NavBrand>
    {/snippet}

    <NavUl>
      <NavLi href="/">Home</NavLi>
      <NavLi href="/components/navbar">Navbar</NavLi>
      <NavLi href="/components/footer">Footer</NavLi>
    </NavUl>
  </Navbar>
  {#snippet codeblock()}
    <HighlightCompo code={modules['./md/default-nav.md'] as string} />
  {/snippet}
</CodeWrapper>

<H2>Breakpoint</H2>
<P>Use the `breakPoint` prop to change the breakpoint. There are 4 breakpoints: `md`, `lg`, `xl`, `xxl`.</P>
<CodeWrapper>
  <Navbar toggleNav={toggleNavLg} closeNav={closeNavLg} navStatus={navStatusLg} breakPoint="lg">
    {#snippet brand()}
      <NavBrand siteName="Svelte 5">
        <img width="30" src="/images/svelte-icon.png" alt="svelte icon" />
      </NavBrand>
    {/snippet}

    <NavUl>
      <NavLi href="/">Home</NavLi>
      <NavLi href="/components/navbar">Navbar</NavLi>
      <NavLi href="/components/footer">Footer</NavLi>
    </NavUl>
  </Navbar>
  {#snippet codeblock()}
    <HighlightCompo code={modules['./md/breakpoint.md'] as string} />
  {/snippet}
</CodeWrapper>

<H2>Sticky navbar</H2>
<CodeWrapper innerClass="p-0">
  <div class="relative">
    <Navbar toggleNav={toggleNav2} closeNav={closeNav2} navStatus={navStatus2} breakPoint="md" navClass="absolute w-full z-20 top-0 start-0 bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-700">
      {#snippet brand()}
        <NavBrand siteName="Svelte 5">
          <img width="30" src="/images/svelte-icon.png" alt="svelte icon" />
        </NavBrand>
      {/snippet}

      <NavUl>
        <NavLi href="/">Home</NavLi>
        <NavLi href="/components/navbar">Navbar</NavLi>
        <NavLi href="/components/footer">Footer</NavLi>
      </NavUl>
    </Navbar>
    <div style="height:300px;" class="overflow-scroll px-8 py-24">
      <Skeleton class="mb-8 mt-16" />
      <ImagePlaceholder class="my-8" />
      <TextPlaceholder class="my-8" />
    </div>
  </div>
  {#snippet codeblock()}
    <HighlightCompo code={modules['./md/sticky-navbar.md'] as string} />
  {/snippet}
</CodeWrapper>

<H2>Component data</H2>
<CompoAttributesViewer {dirName} />
