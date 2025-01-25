<script lang="ts">
	import AppSidebar from '$lib/elements/app-sidebar.svelte';
	import { Separator } from '$lib/components/ui/separator/index.js';
	import * as Sidebar from '$lib/components/ui/sidebar/index.js';

	let input: HTMLDivElement;
	let history: HTMLDivElement;

	// Focus on textarea if the div area is clicked
	function focusToEnd() {
		if (input) {
			const range = document.createRange();
			const selection = window.getSelection();

			// Move the cursor to the end of the content
			range.selectNodeContents(input);
			range.collapse(false); // Collapses the range to the end

			// Apply the selection to move the cursor
			if (selection !== null) {
				selection.removeAllRanges();
				selection.addRange(range);
			}

			// Focus on the contenteditable
			input.focus();
		}
	}

	// Handle enter key
	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			// Prevents a new line in textarea
			event.preventDefault();

			// get text from textarea
			const prevInputChat = input.textContent;

			if (prevInputChat && prevInputChat.trim() === '') {
				return;
			}

			// clear textarea
			input.textContent = '';

			// create new div before the textarea
			const histLine = document.createElement('div');
			histLine.innerHTML = `
                <div class="grid grid-cols-6 pb-4">
					<div class="col-span-5 col-start-2 rounded-xl bg-gray-100 p-4">
                        ${prevInputChat}
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
			<!-- svelte-ignore a11y_no_static_element_interactions -->
			<!-- svelte-ignore a11y_click_events_have_key_events -->
			<div class="col-span-8 col-start-3 rounded-xl bg-gray-100 p-4" onclick={focusToEnd}>
				<!-- <Textarea placeholder="Message ..." class="resize-none" /> -->
				<div class="">
					<div
						id="chat-input"
						class="outline-none"
						contenteditable="true"
						data-virtualkeyboard="true"
						bind:this={input}
						onkeydown={handleKeydown}
					></div>
					<div class="icon"></div>
				</div>
			</div>
		</div>
	</Sidebar.Inset>
</Sidebar.Provider>
