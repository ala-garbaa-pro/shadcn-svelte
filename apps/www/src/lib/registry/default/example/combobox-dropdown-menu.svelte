<script lang="ts">
	import Calendar from "lucide-svelte/icons/calendar";
	import MoreHorizontal from "lucide-svelte/icons/more-horizontal";
	import Tags from "lucide-svelte/icons/tags";
	import Trash from "lucide-svelte/icons/trash";
	import User from "lucide-svelte/icons/user";
	import * as Command from "$lib/registry/default/ui/command/index.js";
	import * as DropdownMenu from "$lib/registry/default/ui/dropdown-menu/index.js";
	import { Button } from "$lib/registry/default/ui/button/index.js";
	import { tick } from "svelte";

	const labels = [
		"feature",
		"bug",
		"enhancement",
		"documentation",
		"design",
		"question",
		"maintenance",
	];

	let open = false;
	let selectedLabel = "feature";

	// We want to refocus the trigger button when the user selects
	// an item from the list so users can continue navigating the
	// rest of the form with the keyboard.
	function closeAndFocusTrigger(triggerId: string) {
		open = false;
		tick().then(() => {
			document.getElementById(triggerId)?.focus();
		});
	}
</script>

<div
	class="flex w-full flex-col items-start justify-between rounded-md border px-4 py-3 sm:flex-row sm:items-center"
>
	<p class="text-sm font-medium leading-none">
		<span class="mr-2 rounded-lg bg-primary px-2 py-1 text-xs text-primary-foreground">
			{selectedLabel}
		</span>
		<span class="text-muted-foreground">Create a new project</span>
	</p>
	<DropdownMenu.Root bind:open let:ids>
		<DropdownMenu.Trigger asChild let:builder>
			<Button builders={[builder]} variant="ghost" size="sm" aria-label="Open menu">
				<MoreHorizontal />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content class="w-[200px]" align="end">
			<DropdownMenu.Group>
				<DropdownMenu.Label>Actions</DropdownMenu.Label>
				<DropdownMenu.Item>
					<User class="mr-2 h-4 w-4" />
					Assign to...
				</DropdownMenu.Item>
				<DropdownMenu.Item>
					<Calendar class="mr-2 h-4 w-4" />
					Set due date...
				</DropdownMenu.Item>
				<DropdownMenu.Separator />
				<DropdownMenu.Sub>
					<DropdownMenu.SubTrigger>
						<Tags class="mr-2 h-4 w-4" />
						Apply label
					</DropdownMenu.SubTrigger>
					<DropdownMenu.SubContent class="p-0">
						<Command.Root value={selectedLabel}>
							<Command.Input autofocus placeholder="Filter label..." />
							<Command.List>
								<Command.Empty>No label found.</Command.Empty>
								<Command.Group>
									{#each labels as label}
										<Command.Item
											value={label}
											onSelect={(value) => {
												selectedLabel = value;
												closeAndFocusTrigger(ids.trigger);
											}}
										>
											{label}
										</Command.Item>
									{/each}
								</Command.Group>
							</Command.List>
						</Command.Root>
					</DropdownMenu.SubContent>
				</DropdownMenu.Sub>
				<DropdownMenu.Separator />
				<DropdownMenu.Item class="text-red-600">
					<Trash class="mr-2 h-4 w-4" />
					Delete
					<DropdownMenu.Shortcut>⌘⌫</DropdownMenu.Shortcut>
				</DropdownMenu.Item>
			</DropdownMenu.Group>
		</DropdownMenu.Content>
	</DropdownMenu.Root>
</div>
