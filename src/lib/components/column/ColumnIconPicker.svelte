<script lang="ts">
  import { iconMap } from '$lib/columnIcons';
  import { fly, fade } from 'svelte/transition';
  import {createEventDispatcher} from "svelte";
  const dispatch = createEventDispatcher();

  let { current } = $props();

  function changeIcon(key) {
      dispatch('change', {
          icon: key,
      });
  }

  function close() {
      dispatch('close');
  }
</script>

<div class="column-icon-picker" transition:fly={{ duration:250, y: -30 }}>
  <ul class="icon-picker-list">
    {#each iconMap as [key, icon]}
      {@const SvelteComponent = icon}
      <button
          class="icon-picker-list__button"
          class:icon-picker-list__button--current={key === current}
          onclick={() => {changeIcon(key)}}
      >
        <SvelteComponent color="var(--text-color-1)"></SvelteComponent>
      </button>
    {/each}
  </ul>
</div>

<button class="column-icon-picker-bg" transition:fade={{ duration: 150 }} onclick={close} aria-label="Close"></button>

<style lang="postcss">
  .column-icon-picker {
      position: absolute;
      background-color: var(--bg-color-1);
      padding: 16px;
      top: 60px;
      left: 16px;
      right: 16px;
      box-shadow: 0 0 10px var(--box-shadow-color-1);
      border-radius: 6px;
      z-index: 1;
  }

  .icon-picker-list {
      list-style: none;
      display: flex;
      flex-wrap: wrap;
      gap: 5px;

      &__button {
          aspect-ratio: 1 / 1;
          display: grid;
          place-content: center;
          width: 36px;
          border-radius: 4px;

          &--current {
              border: 2px solid var(--primary-color);
          }

          &:hover {
              background-color: var(--bg-color-3);
          }
      }
  }

  .column-icon-picker-bg {
      outline: none;
      position: absolute;
      top: 52px;
      left: 0;
      right: 0;
      height: calc(100dvh - 52px);
      background-color: rgba(255, 255, 255, 0);
      backdrop-filter: blur(5px);
  }
</style>