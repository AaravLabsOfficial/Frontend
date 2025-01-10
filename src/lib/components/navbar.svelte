<script lang="ts">
	import * as Menubar from '$lib/components/ui/menubar';
	import { Button } from '$lib/components/ui/button';
	import { Badge } from '$lib/components/ui/badge';

    let props = $props();
    let currentPage = props.currentPage;
</script>

<div class="pb-6 pl-10 pr-10 pt-6 drop-shadow-glow">
	<Menubar.Root>
		<Menubar.Menu>
			<Button variant="ghost" class="navbarLink {currentPage == "Home" ? "font-bold" : ""}">
				<a href={currentPage == "Home" ? "#" : "/"}>Home</a>
			</Button>
            <Button variant="ghost" class="navbarLink {currentPage == "Practice" ? "font-bold" : ""}">
				<a href={currentPage == "Practice" ? "#" : "/practice"}>Practice</a>
			</Button>
            <Button variant="ghost" class="navbarLink {currentPage == "Lessons" ? "font-bold" : ""}">
                <a href={currentPage == "Lessons" ? "#" : "#"}>Lessons</a>
				<Badge variant="destructive" class="ml-2">Coming Soon</Badge>
			</Button>
		</Menubar.Menu>
	</Menubar.Root>
</div>
