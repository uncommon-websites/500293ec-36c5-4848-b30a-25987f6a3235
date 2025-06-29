<!--
    @component
    Bold hero banner with eye-catching text and strong call-to-action. NEVER use title case.

    Usage:
    ```html
    <Hero
      title="Bold Claim"
      subtitle="Quick Value"
      imageSrc="/hero.jpg"
      fullWidthImageBelow={true}
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
    - `centered`: Center the content (boolean, default: false)
    - `fullWidthImageBelow`: Show image full width below CTA (boolean, default: false)
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
		fullWidthImageBelow?: boolean;
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
		fullWidthImageBelow = false,
		...rest
	}: Props = $props();
</script>

<div class="bg-background section-py" {...rest}>
	{#if fullWidthImageBelow}
		<!-- Full width image below CTA layout -->
		<div class="section-px container mx-auto" data-enter-container>
			<!-- Content -->
			<div class="grid gap-8 text-center">
				<h1 class="text-display font-[500] tracking-tight" data-enter>
					<span class="mb-4 block">{title}</span>
					<span class="text-emphasis-dim text-title2 mt-2 block">{subtitle}</span>
				</h1>

				{#if callsToAction.length > 0}
					<div class="flex flex-wrap gap-4 justify-center" data-enter>
						{#each callsToAction as cta, index}
							<Button
								href={cta.href}
								size="lg"
								variant={index % 2 === 0 ? "primary" : "secondary"}
								class="rounded-[var(--radius-md)] px-8 py-3 font-[450] shadow-md transition hover:shadow-lg focus:ring-2 focus:ring-[var(--color-primary)] focus:outline-none max-lg:hidden"
								>{cta.label}</Button
							>
							<Button
								href={cta.href}
								size="md"
								variant={index % 2 === 0 ? "primary" : "secondary"}
								class="rounded-[var(--radius-md)] px-6 py-2 font-[450] shadow transition hover:shadow-lg focus:ring-2 focus:ring-[var(--color-primary)] focus:outline-none lg:hidden"
								>{cta.label}</Button
							>
						{/each}
					</div>
				{/if}
			</div>
		</div>

		<!-- Full width image -->
		{#if imageSrc}
			<div class="w-full mt-12 px-4 sm:px-6 lg:px-8" data-enter>
				<img
					src={imageSrc}
					alt="Hero image"
					class="w-full h-auto object-cover"
					onerror={handleImageError}
				/>
			</div>
		{/if}
	{:else if imageSrc && !centered}
		<!-- Split layout with image -->
		<div class="section-px container mx-auto" data-enter-container>
			<div class="grid items-center gap-12 lg:grid-cols-2 lg:gap-20">
				<!-- Content -->
				<div class="grid gap-8">
					<h1 class="text-display font-[500] tracking-tight" data-enter>
						<span class="mb-4 block">{title}</span>
						<span class="text-emphasis-dim text-title2 mt-2 block">{subtitle}</span>
					</h1>

					{#if callsToAction.length > 0}
						<div class="flex flex-wrap gap-4" data-enter>
							{#each callsToAction as cta, index}
								<Button
									href={cta.href}
									size="lg"
									variant={index % 2 === 0 ? "primary" : "secondary"}
									class="rounded-[var(--radius-md)] px-8 py-3 font-[450] shadow-md transition hover:shadow-lg focus:ring-2 focus:ring-[var(--color-primary)] focus:outline-none max-lg:hidden"
									>{cta.label}</Button
								>
								<Button
									href={cta.href}
									size="md"
									variant={index % 2 === 0 ? "primary" : "secondary"}
									class="rounded-[var(--radius-md)] px-6 py-2 font-[450] shadow transition hover:shadow-lg focus:ring-2 focus:ring-[var(--color-primary)] focus:outline-none lg:hidden"
									>{cta.label}</Button
								>
							{/each}
						</div>
					{/if}
				</div>

				<!-- Image -->
				<div
					class="aspect-[4/3] overflow-hidden rounded-[var(--radius-lg)] shadow-lg lg:order-last"
					data-enter
				>
					<img
						src={imageSrc}
						alt="Hero image"
						class="size-full object-cover"
						onerror={handleImageError}
					/>
				</div>
			</div>
		</div>
	{:else}
		<!-- Centered layout or no image -->
		<header
			class={[
				"section-px container mx-auto grid items-end gap-20 gap-y-12 text-balance",
				centered ? "place-items-center text-center xl:grid-cols-1" : "xl:grid-cols-[1fr_auto]"
			]}
			data-enter-container
		>
			<div class={["grid gap-8", centered ? "max-w-prose mx-auto" : "max-w-2xl lg:max-w-3xl"]}>
				<h1 class="text-display w-full font-[500] tracking-tight text-center" data-enter>
					<span class="mb-4 block">{title}</span>
					{#if !centered}
						<span class="text-emphasis-dim text-title2 mt-2 block">{subtitle}</span>
					{/if}
				</h1>

				{#if centered}
					<p
						data-enter
						class={[
							"text-muted-foreground text-headline mx-auto mt-2 block max-w-[45ch] transition duration-500 ease-out"
						]}
					>
						{subtitle}
					</p>
				{/if}
			</div>

			{#if callsToAction.length > 0}
				<div class={["mt-8 flex flex-wrap gap-4", centered ? "justify-center" : ""]} data-enter>
					{#each callsToAction as cta, index}
						<Button
							href={cta.href}
							size="lg"
							variant={index % 2 === 0 ? "primary" : "secondary"}
							class="rounded-[var(--radius-md)] px-8 py-3 font-[450] shadow-md transition hover:shadow-lg focus:ring-2 focus:ring-[var(--color-primary)] focus:outline-none max-lg:hidden"
							>{cta.label}</Button
						>
						<Button
							href={cta.href}
							size="md"
							variant={index % 2 === 0 ? "primary" : "secondary"}
							class="rounded-[var(--radius-md)] px-6 py-2 font-[450] shadow transition hover:shadow-lg focus:ring-2 focus:ring-[var(--color-primary)] focus:outline-none lg:hidden"
							>{cta.label}</Button
						>
					{/each}
				</div>
			{/if}
		</header>

		{#if imageSrc && centered}
			<div
				class="section-px container mx-auto mt-12 aspect-video overflow-hidden rounded-[var(--radius-lg)] shadow-lg"
				data-enter
			>
				<img
					src={imageSrc}
					alt="Hero image"
					class="size-full object-cover"
					onerror={handleImageError}
				/>
			</div>
		{/if}
	{/if}
</div>
