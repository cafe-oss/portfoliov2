<script>
    import { process } from '../data/process.js';
</script>

{#each process as item, index}
    
    <div class="process-card bg-gradient-to-b from-[#FCFCFD] to-[#F8F8FA00] rounded-default p-8 shadow-default transition-all duration-300 hover:-translate-y-1 hover:shadow-hover">
            <div class="step-number text-secondary text-[48px] font-bold mb-4 opacity-30">0{index + 1}</div>
            <h3 class="text-[22px] font-semibold mb-3">{item.title}</h3>
            <p class="text-text-light mb-4">
                {item.description}
            </p>
            <ul class="space-y-2 text-sm text-text-light">
                {#each item.list as listItem}
                    <li class="flex items-start gap-2">
                        <span class="text-secondary">•</span>
                        <span>{listItem}</span>
                    </li>
                {/each}
            </ul>
        </div>
{/each}