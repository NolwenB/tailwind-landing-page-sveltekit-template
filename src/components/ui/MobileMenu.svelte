<script lang="ts">
  import { writable } from 'svelte/store';
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition';
	import { clickOutside } from '$lib/click-outside';
	import IconBullets from './images/IconBullets.svelte';
	import Navigation from './navigation/Navigation.svelte';

  let mobileNavOpen = writable(false);
  let trigger: HTMLButtonElement; // Declare trigger
  let mobileNav: HTMLElement; // Declare mobileNav

  // Event Handlers
  function handleKeyDown(event: KeyboardEvent) {
    if (event.key === 'Escape') {
      mobileNavOpen.set(false);
    }
  }

  // Toggle mobile nav
  function toggleMobileNav() {
    mobileNavOpen.update(n => !n);
  }
  function closeMobileNav() {
    mobileNavOpen.set(false);
  }
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div use:clickOutside on:keydown={handleKeyDown} on:clickOutside={() => mobileNavOpen.set(false)} class="flex md:hidden">
  <!-- Hamburger button -->
  <button
    bind:this={trigger}
    class="hamburger"
    class:active={$mobileNavOpen}
    aria-controls="mobile-nav"
    aria-expanded={$mobileNavOpen}
    on:click={toggleMobileNav}
  >
    <span class="sr-only">Menu</span>
    <svg class="w-6 h-6 fill-current text-gray-900" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
      <rect y="4" width="24" height="2" />
      <rect y="11" width="24" height="2" />
      <rect y="18" width="24" height="2" />
    </svg>
  </button>

  <!-- Mobile navigation -->
  {#if $mobileNavOpen}
    <nav
      bind:this={mobileNav}
      id="mobile-nav"
      class="absolute top-full h-screen pb-16 z-20 left-0 w-full overflow-scroll bg-white"
      transition:fade={{ duration: 200 }}
    >
      <!-- Nav content -->
      <Navigation isMobile={true} class="px-5 py-2" />
  
    </nav>
  {/if}
</div>
