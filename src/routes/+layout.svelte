<script lang="ts">
	import favicon from "$lib/assets/favicon.svg";
	import Nav from "$lib/MyLayout/Nav.svelte";
	import SideBar from "$lib/MyLayout/SideBar.svelte";
	import CourseFormDrawer from "$lib/compornent/CourseFormDrawer.svelte";
	import "./app.css";

	import { openCourseForm } from "$lib/stores/courseForm";

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>
<div>
	<Nav />

	<div class="flex">
		<SideBar />
		<div class="p-12 w-full">
			{@render children()}
		</div>
	</div>

	<CourseFormDrawer />
</div>
