<script lang="ts">
	import type { Media } from '@prisma/client';
	import CoolTextOnHover from './CoolTextOnHover.svelte';
	import NeovimPluginMetaData from './NeovimPluginMetaData.svelte';
	import { getMediaType } from '$lib/utils';
	// TODO: Refactor to title isntead of username/name to be more input agnostic
	export let username: string;
	export let name: string;
	export let stars: string;
	export let configCount: number;
	export let category: string;
	export let description: string;
	export let thumbnail: Media | null = null;
</script>

<div
	class="h-full flex flex-col justify-between rounded-md bg-white/10 hover:bg-white/20 transition-colors shadow-lg p-2 pl-5"
>
	<div class="h-full flex space-x-4 transition-colors items-start min-h-[8rem]">
		<!-- min-h-[8rem] corresponds to h-32 -->
		<div class="w-full h-full flex flex-col gap-4">
			<div class="flex flex-col gap-1 h-full">
				<div class="flex w-full">
					<CoolTextOnHover>
						<a
							href={`/plugins/${username}/${name}`}
							class="text-sm font-semibold tracking-wide md:text-base whitespace-normal"
						>
							{username}/{name}
						</a>
					</CoolTextOnHover>
				</div>
				<span class="text-xs font-medium tracking-wide md:text-sm whitespace-normal">
					{description}
				</span>
			</div>
		</div>
		{#if thumbnail}
			<div class="h-22 w-72 rounded flex items-center">
				{#if getMediaType(thumbnail) === 'image'}
					<img
						class="h-full w-full object-cover rounded-lg text-xs max-h-[7rem]"
						on:error={() => console.log('failed loading thumbnail')}
						style="object-position: center;"
						src={thumbnail.url}
						alt="{name} thumbnail"
					/>
				{/if}
				{#if getMediaType(thumbnail) === 'video'}
					<video
						class="h-full w-full object-cover rounded-lg text-xs max-h-[7rem]"
						on:error={() => console.log('failed loading thumbnail')}
						style="object-position: center;"
						src={thumbnail.url}
					/>
				{/if}
			</div>
		{/if}
	</div>
	<!-- TODO: refactor to be a slot  -->
	<NeovimPluginMetaData {stars} {configCount} {category} />
</div>