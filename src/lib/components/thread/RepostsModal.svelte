<script>
    import {_} from "svelte-i18n";
    import {createEventDispatcher} from 'svelte';
    import { agent } from "$lib/stores";
    const dispatch = createEventDispatcher();
    import InfiniteLoading from 'svelte-infinite-loading';
    import UserItem from "../../../routes/(app)/profile/[handle]/UserItem.svelte";
    import Modal from "$lib/components/ui/Modal.svelte";

    let { uri, _agent = $agent } = $props();
    let reposts = $state([]);
    let cursor;

    async function handleLoadMore({ detail: { loaded, complete } }) {
        try {
            const res = await _agent.agent.api.app.bsky.feed.getRepostedBy({uri: uri, cursor: cursor});
            cursor = res.data.cursor;
            reposts = [...reposts, ...res.data.repostedBy];

            if (cursor) {
                loaded();
            } else {
                complete();
            }
        } catch (e) {
            console.error(e);
            complete();
        }
    }
</script>

<Modal title="{$_('reposted_users')}" size="small" on:close>
    <div class="likes">
        {#each reposts as repost }
            {#if (!repost.viewer?.muted)}
                <UserItem user={repost}></UserItem>
            {/if}
        {/each}
    </div>

    <InfiniteLoading on:infinite={handleLoadMore}>
        {#snippet noMore()}
                <p  class="infinite-nomore">もうないよ</p>
            {/snippet}
        {#snippet noResults()}
                <p ></p>
            {/snippet}
    </InfiniteLoading>
</Modal>
