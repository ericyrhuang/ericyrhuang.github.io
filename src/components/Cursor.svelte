<script lang="ts">
  import { onMount } from 'svelte';

  let cursorX = -100;
  let cursorY = -100;
  let isHovering = false;
  let isVisible = false;
  let hasMovedOnce = false;

  onMount(() => {
    // Only show on desktop (>= 992px)
    const checkViewport = () => {
      isVisible = window.innerWidth >= 992;
    };

    checkViewport();
    window.addEventListener('resize', checkViewport);

    // Track mouse movement
    const handleMouseMove = (e: MouseEvent) => {
      cursorX = e.clientX;
      cursorY = e.clientY;
      if (!hasMovedOnce) {
        hasMovedOnce = true;
      }
    };

    // Detect hover on interactive elements
    const handleMouseEnter = () => {
      isHovering = true;
    };

    const handleMouseLeave = () => {
      isHovering = false;
    };

    document.addEventListener('mousemove', handleMouseMove);

    // Add listeners to all links and buttons
    const interactiveElements = document.querySelectorAll('a, button');
    interactiveElements.forEach((el) => {
      el.addEventListener('mouseenter', handleMouseEnter);
      el.addEventListener('mouseleave', handleMouseLeave);
    });

    return () => {
      window.removeEventListener('resize', checkViewport);
      document.removeEventListener('mousemove', handleMouseMove);
      interactiveElements.forEach((el) => {
        el.removeEventListener('mouseenter', handleMouseEnter);
        el.removeEventListener('mouseleave', handleMouseLeave);
      });
    };
  });
</script>

{#if isVisible}
  <div
    class="cursor-outer"
    class:hovering={isHovering}
    class:visible={hasMovedOnce}
    style="left: {cursorX}px; top: {cursorY}px;"
  >
    <div class="cursor-inner" class:hovering={isHovering}></div>
  </div>
{/if}

<style>
  .cursor-outer {
    position: fixed;
    width: 40px;
    height: 40px;
    border: 2px solid #222222;
    border-radius: 50%;
    background: rgba(235, 235, 235, 0.2);
    pointer-events: none;
    transform: translate(-50%, -50%);
    opacity: 0;
    transition:
      width 0.2s ease-out,
      height 0.2s ease-out,
      background 0.2s ease-out,
      opacity 0.2s ease-out;
    z-index: 9999;
  }

  .cursor-outer.visible {
    opacity: 1;
  }

  .cursor-outer.hovering {
    width: 35px;
    height: 35px;
  }

  .cursor-inner {
    position: absolute;
    width: 10px;
    height: 10px;
    background: #222222;
    border-radius: 50%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    transition:
      width 0.2s ease-out,
      height 0.2s ease-out;
  }

  .cursor-inner.hovering {
    width: 14px;
    height: 14px;
  }
</style>
