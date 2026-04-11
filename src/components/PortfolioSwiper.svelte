<script>
    import { onMount } from "svelte";
    import { register } from "swiper/element/bundle";

    export let portfolios = [];

    let isDesktop = true;
    let swiperEl;

    onMount(() => {
        register();

        const mediaQuery = window.matchMedia("(min-width: 768px)");

        isDesktop = mediaQuery.matches;

        const handleResize = (e) => {
            isDesktop = e.matches;
        };

        mediaQuery.addEventListener("change", handleResize);

        return () => mediaQuery.removeEventListener("change", handleResize);
    });
function handleHover() {
    if (isDesktop && swiperEl) {
      swiperEl.swiper.autoplay.stop();
    }
  }

  function handleLeave() {
    if (isDesktop && swiperEl) {
      swiperEl.swiper.autoplay.start();
    }
  }
</script>

<swiper-container
    bind:this={swiperEl}
    slides-per-view="auto"
    space-between="24"
    loop="true"
    grab-cursor="false"
    speed={isDesktop ? "5000" : "6000"}
    autoplay-delay={isDesktop ? "0" : "3000"}
    centered-slides={!isDesktop ? "true" : "false"}
    autoplay-disable-on-interaction="false"
  on:mouseenter={handleHover}
  on:mouseleave={handleLeave}
    class={`w-full py-4 flex overflow-hidden ${isDesktop ? 'is-marquee' : ''}`}
>
    {#each portfolios as port, index}
    <swiper-slide class="max-w-80 shrink-0">
      <div class="h-56 bg-gray-800 rounded-xl overflow-hidden shadow-lg">
        <img src={port} alt={`Portfolio ${index + 1}`} class="w-full h-full object-cover" />
      </div>
    </swiper-slide>
    {/each}
</swiper-container>

<style>
    swiper-container.is-marquee::part(wrapper) {
        transition-timing-function: linear !important;
    }
</style>
