<script lang="ts">
  import Link, { parseURL } from '$lib/components/input/Link.svelte'
  import { Material } from 'mono-svelte'
  import { Icon, Link as LinkIcon } from 'svelte-hero-icons'

  export let url: string
  export let thumbnail_url: string | undefined = undefined
  export let nsfw: boolean = false
  export let embed_title: string | undefined = undefined
  export let embed_description: string | undefined = undefined
  export let compact: boolean = false

  $: richURL = parseURL(url)
</script>

{#if embed_title && !compact}
  <Material color="distinct" class="flex flex-row gap-4">
    <div class="flex flex-col gap-2">
      {#if richURL}
        <Link
          href={url}
          class="text-slate-600 dark:text-zinc-400 inline-flex items-center gap-1 text-xs"
        >
          <Icon src={LinkIcon} size="12" mini slot="icon" />
          {richURL.hostname}
        </Link>
      {/if}
      <a href={url} class="font-medium text-base hover:underline">
        {embed_title}
      </a>
      {#if embed_description}
        <p class="text-sm">
          {embed_description.slice(0, 300)}{embed_description.length > 300
            ? '...'
            : ''}
        </p>
      {/if}
    </div>
    {#if thumbnail_url}
      <a
        href={url}
        class="ml-auto w-full thumbnail rounded-r-lg overflow-hidden flex-shrink -m-4"
      >
        <img
          src={thumbnail_url}
          class="w-full h-full object-cover bg-slate-200 dark:bg-zinc-800"
          width={600}
          height={400}
          alt=""
          class:blur-3xl={nsfw}
        />
      </a>
    {/if}
  </Material>
{:else}
  <Link href={url} highlight nowrap />
  {#if thumbnail_url && !compact}
    <a href={url}>
      <img
        src={thumbnail_url}
        class="rounded-md max-w-[300px] w-full h-auto aspect-video object-cover bg-slate-200 dark:bg-zinc-800"
        width={600}
        height={400}
        alt=""
        class:blur-3xl={nsfw}
      />
    </a>
  {/if}
{/if}

<style>
  .thumbnail {
    max-width: 240px;
    height: calc(100%+16px);
  }
</style>
