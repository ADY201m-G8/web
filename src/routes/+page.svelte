<script lang="ts">
	import { env } from '$env/dynamic/public';
	import { onMount } from 'svelte';

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

<div class="flex flex-wrap gap-12 p-12">
	{#each attendance as att (att.id)}
		<div class={['border', att.present == 1 ? 'bg-green-200' : '']}>
			<p>{att.id}</p>
			<p>{att.fullname}</p>
		</div>
	{/each}
</div>

<button class="m-12 border p-4" onclick={fetchAttendance}>Fetch</button>
