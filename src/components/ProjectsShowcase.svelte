
<script>
    import { onMount } from 'svelte';
    import { projects } from '../data/projects.js';

    let currentIndex = 0;

    onMount(() => {
        if (!window.gsap || !window.ScrollTrigger) {
            console.error('GSAP or ScrollTrigger not loaded.');
            return;
        }

        const gsap = window.gsap;
        const ScrollTrigger = window.ScrollTrigger;

        gsap.registerPlugin(ScrollTrigger);

        const projectInfos = gsap.utils.toArray('.project-info');
        const projectImages = gsap.utils.toArray('.project-image');
        const projectCard = document.querySelector('.project-card');

        if (
            !projectCard ||
            !projectInfos.length ||
            projectInfos.length !== projectImages.length
        ) {
            return;
        }

        const mm = gsap.matchMedia();

        mm.add('(min-width: 769px)', () => {
            let activeIndex = 0;

            // Set consistent initial states.
            projectImages.forEach((image, index) => {
                gsap.set(image, {
                    opacity: index === 0 ? 1 : 0,
                    scale: index === 0 ? 1 : 0.95,
                    filter: index === 0
                        ? 'blur(0px)'
                        : 'blur(12px)',
                    zIndex: index === 0 ? 10 : 1
                });
            });

            function switchImage(newIndex) {
                if (
                    newIndex === activeIndex ||
                    newIndex < 0 ||
                    newIndex >= projectImages.length
                ) {
                    return;
                }

                activeIndex = newIndex;
                currentIndex = newIndex;

                // Stop previous transitions before starting another.
                gsap.killTweensOf(projectImages);

                projectImages.forEach((image, index) => {
                    if (index !== newIndex) {
                        gsap.set(image, {
                            opacity: 0,
                            scale: 0.95,
                            filter: 'blur(12px)',
                            zIndex: 1
                        });
                    }
                });

                const nextImage = projectImages[newIndex];

                gsap.set(nextImage, {
                    opacity: 0,
                    scale: 1.05,
                    filter: 'blur(12px)',
                    zIndex: 10
                });

                gsap.to(nextImage, {
                    opacity: 1,
                    scale: 1,
                    filter: 'blur(0px)',
                    duration: 0.45,
                    ease: 'power2.out',
                    overwrite: true
                });
            }

            function getActiveIndex() {
                const viewportCenter = window.innerHeight / 2;
                let closestIndex = 0;
                let closestDistance = Infinity;

                projectInfos.forEach((info, index) => {
                    const rect = info.getBoundingClientRect();
                    const center = rect.top + rect.height / 2;
                    const distance = Math.abs(
                        center - viewportCenter
                    );

                    if (distance < closestDistance) {
                        closestDistance = distance;
                        closestIndex = index;
                    }
                });

                return closestIndex;
            }

            function syncActiveProject() {
                switchImage(getActiveIndex());
            }

            const trigger = ScrollTrigger.create({
                trigger: projectCard,
                start: 'top center',
                end: 'bottom center',
                invalidateOnRefresh: true,

                onEnter: syncActiveProject,
                onEnterBack: syncActiveProject,
                onUpdate: syncActiveProject,
                onRefresh: syncActiveProject,

                onLeaveBack: () => {
                    switchImage(0);
                },

                onLeave: () => {
                    switchImage(projectImages.length - 1);
                }
            });

            ScrollTrigger.refresh();

            return () => {
                trigger.kill();
                gsap.killTweensOf(projectImages);
                gsap.set(projectImages, { clearProps: 'all' });
            };
        });

        return () => {
            mm.revert();
        };
    });
</script>


<section class="projects py-20" id="work">
      <div class="projects-header text-center max-w-[700px] mx-auto mb-[60px]">
          <h2 class="text-[42px] font-bold mb-4">My Work Experiences</h2>
          <p class="text-text-light">With over three years of hands-on expertise in WordPress, WooCommerce.</p>
      </div>

      <div class="projects-grid">
          <div class="project-card flex gap-10 bg-gradient-to-b from-[#FCFCFD] to-[#F8F8FA00] rounded-default p-10 shadow-default transition-all duration-300">
              <!-- Left: Project Infos -->
              <div class="project-infos flex-[0_0_40%] flex flex-col">
                  {#each projects as project, index}
                      <div class="project-info min-h-screen flex flex-col justify-center opacity-30 transition-opacity duration-300" class:active={index === currentIndex}>
                          {#if project.mediaType === 'video'}
                                <video playsinline muted loop autoplay
                                    class="w-full rounded-2xl mb-6 min-[769px]:hidden"
                                >
                                    <source src={project.image} type="video/mp4">
                                </video>
                            {:else}
                                <img
                                    src={project.image}
                                    alt={project.title}
                                    class="w-full h-auto object-cover rounded-2xl mb-6 min-[769px]:hidden"
                                >
                            {/if}
                          <h3 class="text-[28px] md:text-[32px] font-semibold mb-4">{project.title}</h3>
                          <p class="text-text-light mb-5 text-sm md:text-base">{project.description}</p>

                          <div class="project-tags flex flex-wrap gap-2.5 mb-5">
                              {#each project.tags as tag}
                                  <span class="bg-secondary/10 text-secondary px-3 py-1.5 md:px-4 md:py-2 rounded-default text-xs">
                                      {tag}
                                  </span>
                              {/each}
                          </div>

                          {#if project.link}
                              <div class="project-link mt-4">
                                  <a
                                      href={project.link}
                                      target="_blank"
                                      rel="noopener noreferrer"
                                      class="inline-flex items-center gap-2 text-secondary font-medium hover:gap-3 transition-all duration-300 text-sm md:text-base"
                                  >
                                      View Live Site
                                      <span class="text-sm">→</span>
                                  </a>
                              </div>
                          {/if}
                      </div>
                  {/each}
              </div>

              <!-- Right: Project Images (Sticky) -->
              <div class="project-images hidden min-[769px]:flex flex-1 self-start sticky top-[100px] h-[calc(100vh-140px)] max-h-[680px] items-center justify-center">
                  <div class="project-images-wrapper relative w-full h-full">
                      {#each projects as project, index}
                          <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[90%] max-w-full h-auto max-h-[90%]">
                            {#if project.mediaType === 'video'}
                                <video
                                    playsinline muted loop autoplay
                                    class="project-image w-full h-auto object-cover rounded-2xl shadow-[0_20px_60px_rgba(0,0,0,0.15)]"
                                >
                                    <source src={project.image} type="video/mp4">
                                </video>
                            {:else}
                                <img
                                    src={project.image}
                                    alt={project.title}
                                    class="project-image w-full h-auto object-cover rounded-2xl shadow-[0_20px_60px_rgba(0,0,0,0.15)]"
                                >
                            {/if}
                        </div>
                      {/each}
                  </div>
              </div>
          </div>
      </div>
  </section>

  <style>
      .project-info.active {
          opacity: 1;
      }

      /* Mobile optimizations */
      @media (max-width: 768px) {
          .projects {
              padding: 60px 0;
          }

          .projects-header h2 {
              font-size: 32px;
          }

          .project-card {
              flex-direction: column;
              padding: 30px 20px !important;
              gap: 40px !important;
          }

          .project-infos {
              flex: 1 !important;
          }

          .project-info {
              min-height: auto !important;
              padding: 40px 0;
              opacity: 1 !important;
          }

          .project-images {
              position: relative !important;
              top: auto !important;
              height: 300px !important;
              order: -1; /* Show image first on mobile */
          }

          .project-image {
              width: 100% !important;
              max-height: 100% !important;
          }
      }

      /* Tablet optimizations */
      @media (min-width: 769px) and (max-width: 1024px) {
          .project-infos {
              flex: 0 0 45% !important;
          }

          .project-card {
              padding: 40px !important;
          }
      }
  </style>
