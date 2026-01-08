<script>
    import { onMount } from 'svelte';
    import { testimonials } from '../data/testimonials.js';

    let columnRefs = [];
    let isInView = false;  // Reactive variable

    onMount(() => {
        // 1. Create observer that watches visibility
        const observer = new IntersectionObserver(
            (entries) => {
                // 2. When visibility changes, update isInView
                entries.forEach((entry) => {
                    isInView = entry.isIntersecting;  // true/false
                });
            },
            { threshold: 0.3 }  // Trigger at 30% visible
        );

        // 3. Find the element to watch
        const section = document.querySelector('.testimonials');

        // 4. Start watching
        if (section) {
            observer.observe(section);
        }

        // 5. Cleanup when component destroyed
        return () => {
            observer.disconnect();  // Stop watching
        };
    });
</script>


<section class="testimonials py-20 rounded-default">
    <h2 class="text-[42px] font-bold text-center max-w-[640px] mx-auto mb-[60px]">
        Relied upon by a Fresh Generation of Companies
    </h2>

    <div class="testimonials-grid flex gap-6">
        {#each testimonials as column, columnIndex}
            <div
                class="testimonial-column flex flex-col gap-2.5 transition-transform duration-[400ms] ease-[cubic-bezier(0.25,0.46,0.45,0.94)]"
                class:translate-y-[-32px]={isInView && columnIndex % 2 === 0}
                class:translate-y-[32px]={isInView && columnIndex % 2 === 1}
                bind:this={columnRefs[columnIndex]}
            >
                {#each column as card}
                    <div class="testimonial-card bg-light-bg rounded-default p-10 shadow-default h-[306px] transition-all duration-300 hover:-translate-y-1">
                        <div class="testimonial-stats mb-[30px]">
                            <span class="stat-number text-[60px] font-bold text-secondary block leading-none">{card.stat}</span>
                            <span class="stat-label text-base text-text-light">{card.label}</span>
                        </div>

                        <div class="testimonial-author flex flex-col gap-1.5">
                            <span class="author-name font-bold">{card.name}</span>
                            <span class="author-role text-text-light text-sm">{card.role}</span>
                        </div>

                        <div class="testimonial-logo mt-[30px]">
                            <img src={card.logo} alt="Company logo" class="h-10 w-auto invert">
                        </div>
                    </div>
                {/each}
            </div>
        {/each}
    </div>
</section>

<style>
    .testimonials {
        background: linear-gradient(180deg, #f9f7f600 29.8661%, #f9f7f6 100%);
    }
</style>