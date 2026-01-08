<script>
    import { onMount } from 'svelte';
    import { projects } from '../data/projects.js';

    let currentIndex = 0;

    onMount(() => {
        // Wait for GSAP to load from CDN
        if (typeof window.gsap === 'undefined' || typeof window.ScrollTrigger === 'undefined') {
            console.error('GSAP not loaded yet! Make sure scripts are in Layout.astro');
            return;
        }

        const gsap = window.gsap;
        const ScrollTrigger = window.ScrollTrigger;

        gsap.registerPlugin(ScrollTrigger);

        const projectInfos = document.querySelectorAll('.project-info');
        const projectImages = document.querySelectorAll('.project-image');

        // Use matchMedia to only apply animations on desktop (769px and up)
        const mm = gsap.matchMedia();

        mm.add("(min-width: 769px)", () => {
            // Set initial state - show first image
            projectImages.forEach((img, index) => {
                if (index === 0) {
                    gsap.set(img, {
                        scale: 1,
                        opacity: 1,
                        filter: 'blur(0px)',
                        zIndex: 10
                    });
                } else {
                    gsap.set(img, {
                        scale: 0.9,
                        opacity: 0,
                        filter: 'blur(15px)',
                        zIndex: 1
                    });
                }
            });

            // Function to switch images
            function switchImage(newIndex) {
                if (newIndex === currentIndex) return;

                const currentImage = projectImages[currentIndex];
                const newImage = projectImages[newIndex];

                // Set z-index for layering
                gsap.set(currentImage, { zIndex: 9 });
                gsap.set(newImage, { zIndex: 10 });

                // Create timeline for smooth transition
                const tl = gsap.timeline({
                    onComplete: () => {
                        gsap.set(currentImage, { zIndex: 1 });
                    }
                });

                // Animate out current image
                tl.to(currentImage, {
                    scale: 0.9,
                    opacity: 0,
                    filter: 'blur(15px)',
                    duration: 0.3,
                    ease: 'power2.in'
                }, 0);

                // Animate in new image
                tl.fromTo(newImage,
                    {
                        scale: 1.1,
                        opacity: 0,
                        filter: 'blur(15px)'
                    },
                    {
                        scale: 1,
                        opacity: 1,
                        filter: 'blur(0px)',
                        duration: 0.5,
                        ease: 'power2.out'
                    }, 0.15);

                currentIndex = newIndex;
            }

            // Create ScrollTrigger for each project info
            projectInfos.forEach((info, index) => {
                ScrollTrigger.create({
                    trigger: info,
                    start: 'top 60%',
                    end: 'bottom 40%',
                    onEnter: () => switchImage(index),
                    onEnterBack: () => switchImage(index),
                    markers: false  // Set to true for debugging
                });
            });

            // Pin the images container
            ScrollTrigger.create({
                trigger: '.project-card',
                start: 'top 100px',
                end: 'bottom bottom',
                pin: '.project-images',
                pinSpacing: false
            });
        });

        // Cleanup
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
                          
                          <img
                                src={project.image}
                                alt={project.title}
                                class="w-full h-auto object-cover rounded-2xl mb-6 lex min-[769px]:hidden "
                            >
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
              <div class="project-images hidden min-[769px]:flex flex-1 sticky top-[100px] h-[calc(70vh-200px)] items-center justify-center ">
                  <div class="project-images-wrapper relative w-full h-full">
                      {#each projects as project, index}
                          <!-- Wrapper handles positioning, GSAP animates inner image -->
                          <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[90%] max-w-full h-auto max-h-[90%]">
                              <img
                                  src={project.image}
                                  alt={project.title}
                                  class="project-image w-full h-auto object-cover rounded-2xl shadow-[0_20px_60px_rgba(0,0,0,0.15)]"
                              >
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
              max-h: 100% !important;
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