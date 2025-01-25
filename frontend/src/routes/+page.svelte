<script lang="ts">
	import AppSidebar from '$lib/elements/app-sidebar.svelte';
	import { Separator } from '$lib/components/ui/separator/index.js';
	import * as Sidebar from '$lib/components/ui/sidebar/index.js';
</script>

<Sidebar.Provider>
	<AppSidebar />
	<Sidebar.Inset>
		<header class="flex h-16 shrink-0 items-center gap-2 border-b px-4">
			<Sidebar.Trigger class="-ml-1" />
			<Separator orientation="vertical" class="mr-2 h-4" />
			New chat
		</header>
		<div class="flex flex-1 flex-col gap-4 p-4"></div>
	</Sidebar.Inset>
</Sidebar.Provider>
