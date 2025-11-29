<script lang="ts">
  export let posts: Array<{
    slug: string;
    title: string;
    description: string;
    date: Date;
    category?: string;
    content?: string;
    type: 'blog' | 'now' | 'misc';
  }> = [];

  let searchQuery = '';
  let showResults = false;

  $: filteredPosts = searchQuery.trim()
    ? posts.filter((post) => {
        const query = searchQuery.toLowerCase();
        const dateStr = formatDate(post.date).toLowerCase();
        return (
          post.title.toLowerCase().includes(query) ||
          post.description.toLowerCase().includes(query) ||
          (post.category && post.category.toLowerCase().includes(query)) ||
          (post.content && post.content.toLowerCase().includes(query)) ||
          dateStr.includes(query)
        );
      })
    : [];

  $: showResults = searchQuery.trim().length > 0;

  function formatDate(date: Date): string {
    return new Intl.DateTimeFormat('en-US', {
      year: 'numeric',
      month: 'short',
      day: 'numeric',
    }).format(new Date(date));
  }

  function getUrl(post: typeof posts[0]): string {
    return `/${post.type}/${post.slug}`;
  }

  function getLabel(post: typeof posts[0]): string {
    return post.category || (post.type === 'now' ? 'Updates' : 'Miscellaneous');
  }
</script>

<div class="search-container">
  <input
    type="text"
    bind:value={searchQuery}
    placeholder="ripgrep"
    class="w-full rounded-lg border border-neutral-300 px-4 py-2 text-neutral-700 transition-colors focus:border-neutral-500 focus:outline-none"
  />

  {#if showResults}
    <div class="mt-2 rounded-lg border border-neutral-200 bg-white">
      {#if filteredPosts.length > 0}
        <div class="divide-y divide-neutral-200">
          {#each filteredPosts as post}
            <a
              href={getUrl(post)}
              class="block px-4 py-3 transition-colors hover:bg-neutral-100"
            >
              <div class="flex items-baseline gap-2">
                <h3 class="font-medium text-black">{post.title}</h3>
                <span class="text-xs text-neutral-400">{getLabel(post)}</span>
              </div>
              <p class="mt-1 text-sm text-neutral-600">{post.description}</p>
              <time class="mt-1 block text-xs text-neutral-500">
                {formatDate(post.date)}
              </time>
            </a>
          {/each}
        </div>
      {:else}
        <p class="px-4 py-3 text-sm italic text-neutral-500">
          No posts found matching "{searchQuery}"
        </p>
      {/if}
    </div>
  {/if}
</div>

<style>
  .search-container {
    position: relative;
    max-width: 400px;
    margin: 0 auto;
  }
</style>
