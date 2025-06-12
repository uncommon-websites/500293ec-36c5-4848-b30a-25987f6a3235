<!--
    @component
    Bold hero banner with eye-catching text and strong call-to-action. NEVER use title case.

    Usage:
    ```html
    <Hero
      title="Bold Claim"
      subtitle="Quick Value"
      imageSrc="/hero.jpg"
      callsToAction={[
        {
          href: "/start",
          label: "Go"
        },
        {
          href: "/learn",
          label: "More"
        }
      ]}
    />
    ```

    Props:
    - `title`: Main headline (string)
    - `subtitle`: Supporting text (string)
    - `imageSrc`: Hero image URL (string)
    - `callsToAction`: CTA buttons array (max two: primary, secondary)
-->

<script lang="ts">
	// Components
	import AnimateText from "$lib/components/animation/AnimateText.svelte";
	import Button from "$lib/components/ui/Button.svelte";

	// Constants
	import { cta } from "$lib/navigation";

	function handleImageError(e: Event) {
		const target = e.currentTarget as HTMLImageElement;
		target.src = "https://placehold.co/800x600/f8fafc/64748b?text=Hero+image";
	}

	// Types
	type Props = {
		centered?: boolean;
		title: string;
		subtitle: string;
		imageSrc?: string;
		callsToAction?: Array<{
			href: string;
			label: string;
		}>; // A maximum of two calls to action, with the first one being primary and the second one being secondary
	};

	let {
		title,
		subtitle,
		imageSrc,
		callsToAction = [cta],
		centered = false,
		...rest
	}: Props = $props();
</script>

<div class="bg-background section-py" {...rest}>
	<header
		class={[
			"section-px container mx-auto grid items-end xl:grid-cols-[1fr_auto] gap-20 gap-y-12 text-balance",
			centered ? "place-items-center text-center" : ""
		]}
		data-enter-container
	>
		<div class="grid gap-8 max-w-2xl lg:max-w-3xl" class:max-w-prose={centered}>
			<h1 class="text-display w-full font-[500] tracking-tight" data-enter>
				<span class="block mb-4">I love canvas comments</span>
				{#if !centered}
					<span class="text-emphasis-dim block text-title2 mt-2"><AnimateText text={subtitle} /></span>
				{/if}
			</h1>

			{#if centered}
				<p
					data-enter
					class={[
						"text-muted-foreground text-headline mx-auto block max-w-[45ch] transition duration-500 ease-out mt-2"
					]}
				>
					{subtitle}
				</p>
			{/if}
		</div>

		{#if callsToAction.length > 0}
			<div class="flex flex-wrap gap-4 mt-8" data-enter>
				{#each callsToAction as cta, index}
					<Button
						href={cta.href}
						size="lg"
						variant={index % 2 === 0 ? "primary" : "secondary"}
						class="rounded-[var(--radius-md)] shadow-md font-[450] px-8 py-3 max-lg:hidden hover:shadow-lg focus:outline-none focus:ring-2 focus:ring-[var(--color-primary)] transition" >{cta.label}</Button
					>
					<Button
						href={cta.href}
						size="md"
						variant={index % 2 === 0 ? "primary" : "secondary"}
						class="rounded-[var(--radius-md)] shadow font-[450] px-6 py-2 lg:hidden hover:shadow-lg focus:outline-none focus:ring-2 focus:ring-[var(--color-primary)] transition" >{cta.label}</Button
					>
				{/each}
			</div>
		{/if}
	</header>

	{#if imageSrc}
		<div class="col-span-full aspect-video rounded-[var(--radius-lg)] overflow-hidden shadow-lg mt-12 lg:mt-0" data-enter>
			<img
				src={imageSrc}
				alt="Customer"
				class="size-full object-cover"
				onerror={handleImageError}
			/>
		</div>
	{/if}
</div>
