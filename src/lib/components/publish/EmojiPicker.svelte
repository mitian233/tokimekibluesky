<script lang="ts">
  import data from '@emoji-mart/data';
  import { Picker } from 'emoji-mart'
  import {createEventDispatcher, onMount} from 'svelte';
  import {locale} from 'svelte-i18n';
  import { clickOutside } from '$lib/clickOutSide';
  import {settings} from "$lib/stores";
  const dispatch = createEventDispatcher();

  let theme = $settings?.design.darkmode === true ? 'dark' : 'light';
  let el = $state();

  function handleEmojiPick(data) {
      dispatch('pick', {
          emoji: data,
      });
  }

  function handleClickOutside(e) {
      dispatch('outside');
  }

  onMount(async () => {
      const pickerOptions = {
          onEmojiSelect: handleEmojiPick,
          data: data,
          locale: $locale,
          theme: theme,
          dynamicWidth: $settings.design?.publishPosition !== 'bottom',
      };
      const picker = new Picker(pickerOptions);

      el.appendChild(picker);
  })
</script>

<div class="emoji-picker-wrap"
     onoutclick={handleClickOutside}
     use:clickOutside={{ignoreElement: '.publish-form-emoji-picker-button'}}
>
  <div bind:this={el}></div>
</div>
