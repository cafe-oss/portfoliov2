<script>
    import { tools } from '../data/tools.js';
</script>

{#each tools as tool, index}
    <div class="tool-item relative rounded-large transition-all duration-300 p-5 shadow-default hover:shadow-[0_15px_30px_rgba(0, 0, 0, 0.1)] w-fit md:w-[40%] lg:w-full lg:hover:-translate-y-[5px]">
        <a href={tool.link} target="_blank" class="group flex flex-col items-center gap-3">
            <img src={tool.image} alt="{tool.title} Logo" class="w-11 h-11 object-contain">
            <span class="text-sm font-medium absolute -top-[34px] left-[-57px] right-[-57px] flex flex-col items-center gap-1 h-min px-1.5 py-1 bg-black/[0.75] text-white rounded-lg overflow-hidden opacity-0 group-hover:opacity-100">{tool.title}</span>
        </a>
    </div>
{/each}

<style>
    .tool-item {
        background: linear-gradient(rgba(252, 252, 253, 0) 0%, rgb(252, 252, 253) 100%);
    }
</style>