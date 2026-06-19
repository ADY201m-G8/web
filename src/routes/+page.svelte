<script lang="ts">
	import { env } from '$env/dynamic/public';
	import Button from '$lib/components/ui/button/button.svelte';
	import { onMount } from 'svelte';
	import { Icon } from '@steeze-ui/svelte-icon';
	import { Refresh, Remove } from '@steeze-ui/material-design-icons';

	type AttendanceItem = {
		id: string;
		fullname: string;
		present: number;
	};

	let attendance = $state.raw<AttendanceItem[]>();

	onMount(async () => {
		await fetchAttendance();
	});

	const fetchAttendance = async () => {
		const res = await fetch(env.PUBLIC_API_ROOT + '/attendance');
		attendance = await res.json();
		console.log(attendance);
	};
</script>

<div class="flex flex-wrap justify-center gap-12 p-12 *:basis-2/12">
	{#each attendance as att (att.id)}
		<div class={['border', att.present == 1 ? 'bg-green-200' : '']}>
			<p>{att.id}</p>
			<p>{att.fullname}</p>
		</div>
	{/each}
</div>

<div class="flex justify-center gap-4">
	<Button variant="outline" size="lg" onclick={fetchAttendance}>
		<Icon src={Refresh} />
		Refresh
	</Button>

	<Button variant="destructive" size="lg" onclick={fetchAttendance}>
		<Icon src={Remove} />
		Uncheck All
	</Button>
</div>
