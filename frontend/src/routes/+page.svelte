<script lang="ts">
	import AppSidebar from '$lib/elements/app-sidebar.svelte';
	import { Separator } from '$lib/components/ui/separator/index.js';
	import * as Sidebar from '$lib/components/ui/sidebar/index.js';
	import { Textarea } from '$lib/components/ui/textarea/index.js';
	import SendIcon from 'lucide-svelte/icons/send';

	let input: HTMLTextAreaElement = document.createElement('textarea');
	let history: HTMLDivElement;

	// resize textarea
	function resize() {
		if (input && input.style) {
			input.style.height = 'auto';
			input.style.height = input.scrollHeight + 'px';
		}
	}
	function handleKeydown(event: KeyboardEvent) {
		if (input === null) return;
		if (event.key === 'Enter' && event.ctrlKey && event.shiftKey) {
			// Prevents a new line in textarea
			event.preventDefault();

			// get text from textarea
			const prevCommand = input.value;

			// clear textarea
			input.value = '';

			// create new div before the textarea
			const histLine = document.createElement('div');
			histLine.innerHTML = `
				<div class="grid grid-cols-6 pb-4">
                    <div class="col-span-5 col-start-2 rounded-xl bg-gray-100 p-4">
                        ${prevCommand}
                    </div>
                </div>
			`;

			// add new div before the textarea
			history.appendChild(histLine);
		}
	}
</script>

<Sidebar.Provider>
	<AppSidebar />
	<Sidebar.Inset>
		<header class="flex h-16 shrink-0 items-center gap-2 border-b px-4">
			<Sidebar.Trigger class="-ml-1" />
			<Separator orientation="vertical" class="mr-2 h-4" />
			New chat
		</header>
		<div id="history" class="grid grid-cols-12 p-4">
			<div class="col-span-8 col-start-3" bind:this={history}>
				<div class="grid-cols-6 pb-4">
					<div class="col-span-6">
						Lorem ipsum dolor sit amet consectetur adipisicing elit. Aut iure minus repellat
						praesentium ipsa sint adipisci commodi eius delectus sequi repellendus nostrum vero
						doloribus esse, reprehenderit temporibus assumenda odit? Quasi.
					</div>
				</div>
				<div class="grid grid-cols-6 pb-4">
					<div class="col-span-5 col-start-2 rounded-xl bg-gray-100 p-4">
						Lorem ipsum dolor sit amet consectetur, adipisicing elit. Similique officia rem mollitia
						quisquam. Aliquid ea explicabo quas ut, nihil maxime nisi earum, dicta voluptatibus
						quibusdam alias quaerat. Aliquid, minima laborum.
					</div>
				</div>
			</div>
		</div>
		<div id="input" class="grid grid-cols-12 p-4">
			<div class="col-span-8 col-start-3 rounded-xl bg-gray-100 p-2">
				<div class="">
					<Textarea
						placeholder="Message ..."
						class="resize-none border-none shadow-none focus-visible:ring-0"
						style="height: 50px;"
						bind:ref={input}
						oninput={resize}
						onkeydown={handleKeydown}
					/>
					<div class="icon">
						<div class="float-left"></div>
						<div class="float-right mb-1 mr-1 rounded-full bg-gray-800 p-2 text-white">
							<SendIcon class="size-5" />
						</div>
					</div>
				</div>
			</div>
		</div>
	</Sidebar.Inset>
</Sidebar.Provider>
