<script>
  import { onMount, onDestroy } from 'svelte';

  /** Nav links. Point hrefs at real routes as pages are added. */
  const links = [
    { label: 'Articles', href: '#' },
    { label: 'Campaigns', href: '#' },
    { label: 'About', href: '#' },
  ];

  let open = $state(false);
  let headerEl;

  /** MediaQueryList for the `md` breakpoint (Tailwind md = 48rem). Created in onMount to stay SSR-safe. */
  let desktopMq;

  const close = () => (open = false);
  const toggle = () => (open = !open);

  const onKeyDown = (e) => {
    if (e.key === 'Escape') close();
  };

  const onPointerDown = (e) => {
    if (headerEl && !headerEl.contains(e.target)) close();
  };

  const onBreakpoint = (e) => {
    // Landing on the desktop layout: the mobile panel is hidden anyway,
    // so reset state so reopening on mobile starts clean.
    if (e.matches) close();
  };

  onMount(() => {
    document.addEventListener('keydown', onKeyDown);
    document.addEventListener('pointerdown', onPointerDown);
    desktopMq = window.matchMedia('(min-width: 48rem)');
    desktopMq.addEventListener('change', onBreakpoint);
  });

  onDestroy(() => {
    // onDestroy also runs during SSR — skip browser cleanup there.
    if (typeof document === 'undefined') return;
    document.removeEventListener('keydown', onKeyDown);
    document.removeEventListener('pointerdown', onPointerDown);
    desktopMq?.removeEventListener('change', onBreakpoint);
  });
</script>

<header
  bind:this={headerEl}
  class="sticky top-0 z-50 border-b border-default bg-accent-500"
>
  <div class="mx-auto flex max-w-4xl items-center justify-between px-6 py-10">
    <!-- Wordmark -->
    <a
      href="/"
      class="text-neutral-0 transition-colors hover:text-neutral-100 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-neutral-0"
      aria-label="Home"
    >
      <svg
        class="h-9 w-auto"
        viewBox="0 0 292 36"
        fill="currentColor"
        xmlns="http://www.w3.org/2000/svg"
        role="img"
        aria-label="HighPeaks DSA"
      >
        <text
          x="0"
          y="30"
          font-family="'Alfa Slab One', serif"
          font-size="30"
          font-weight="400"
        >
          HighPeaks DSA
        </text>
      </svg>
    </a>

    <!-- Desktop nav -->
    <nav
      aria-label="Primary"
      class="hidden items-center gap-6 font-sans text-heading text-neutral-0 md:flex"
    >
      {#each links as link (link.label)}
        <a
          href={link.href}
          class="transition-colors hover:text-neutral-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-neutral-0 rounded-sm"
        >{link.label}</a>
      {/each}
    </nav>

    <!-- Mobile menu toggle -->
    <button
      type="button"
      onclick={toggle}
      aria-expanded={open}
      aria-controls="mobile-nav"
      aria-label={open ? 'Close navigation menu' : 'Open navigation menu'}
      class="flex size-10 items-center justify-center rounded-md text-neutral-0 transition-colors hover:text-neutral-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-neutral-0 md:hidden"
    >
      {#if open}
        <!-- Icon: close -->
        <svg class="size-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true">
          <path d="M6 6l12 12M18 6L6 18" />
        </svg>
      {:else}
        <!-- Icon: menu -->
        <svg class="size-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true">
          <path d="M4 6h16M4 12h16M4 18h16" />
        </svg>
      {/if}
    </button>
  </div>

  <!-- Mobile nav panel -->
  {#if open}
    <nav
      id="mobile-nav"
      aria-label="Primary"
      class="flex animate-slide-in flex-col gap-1 border-t border-default px-6 py-4 font-sans text-body-sm text-neutral-0 motion-reduce:animate-none md:hidden"
    >
      {#each links as link (link.label)}
        <a
          href={link.href}
          onclick={close}
          class="rounded-md px-2 py-2 transition-colors hover:bg-surface-inline hover:text-accent-600 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-neutral-0"
        >{link.label}</a>
      {/each}
    </nav>
  {/if}
</header>
