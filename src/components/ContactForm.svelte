<script>
    let isSubmitting = false;
    let submitSuccess = false;
    let submitError = false;

    async function handleSubmit(event) {
        event.preventDefault();
        isSubmitting = true;
        submitSuccess = false;
        submitError = false;

        const formData = new FormData(event.target);

        try {
            const response = await fetch('https://api.web3forms.com/submit', {
                method: 'POST',
                body: formData
            });

            if (response.ok) {
                submitSuccess = true;
                event.target.reset();

                // Hide success message after 5 seconds
                setTimeout(() => {
                    submitSuccess = false;
                }, 5000);
            } else {
                submitError = true;
            }
        } catch (error) {
            console.error('Form submission error:', error);
            submitError = true;
        } finally {
            isSubmitting = false;
        }
    }
</script>

<section class="contact py-20" id="contact">
    <div class="max-w-[900px] mx-auto">
        <div class="section-header text-center mb-[60px]">
            <h2 class="text-[42px] font-bold mb-4">Let's Work Together</h2>
            <p class="text-text-light max-w-[600px] mx-auto">
                Have a project in mind? I'd love to hear about it. Drop me a message and I'll get back to you within 24 hours.
            </p>
        </div>

        <div class="contact-card bg-gradient-to-b from-[#FCFCFD] to-[#F8F8FA00] rounded-default p-10 shadow-default">
            <form on:submit={handleSubmit}>
                <!-- Access Key -->
                <input
                    type="hidden"
                    name="access_key"
                    value="a925c242-d6a7-41aa-9b04-30addf02d53a"
                />

                <!-- Name Field -->
                <div class="form-group mb-6">
                    <label for="name" class="block text-sm font-semibold mb-2">
                        Name <span class="text-secondary">*</span>
                    </label>
                    <input
                        id="name"
                        type="text"
                        name="name"
                        required
                        disabled={isSubmitting}
                        class="w-full px-5 py-3.5 rounded-default border border-border bg-white text-primary placeholder-text-light focus:outline-none focus:ring-2 focus:ring-secondary/30 focus:border-secondary transition-all duration-300"
                        placeholder="Your name"
                    />
                </div>

                <!-- Email Field -->
                <div class="form-group mb-6">
                    <label for="email" class="block text-sm font-semibold mb-2">
                        Email <span class="text-secondary">*</span>
                    </label>
                    <input
                        id="email"
                        type="email"
                        name="email"
                        required
                        disabled={isSubmitting}
                        class="w-full px-5 py-3.5 rounded-default border border-border bg-white text-primary placeholder-text-light focus:outline-none focus:ring-2 focus:ring-secondary/30 focus:border-secondary transition-all duration-300"
                        placeholder="your.email@example.com"
                    />
                </div>

                <!-- Message Field -->
                <div class="form-group mb-6">
                    <label for="message" class="block text-sm font-semibold mb-2">
                        Message <span class="text-secondary">*</span>
                    </label>
                    <textarea
                        id="message"
                        name="message"
                        required
                        disabled={isSubmitting}
                        class="w-full px-5 py-3.5 rounded-default border border-border bg-white text-primary placeholder-text-light focus:outline-none focus:ring-2 focus:ring-secondary/30 focus:border-secondary transition-all duration-300 resize-none"
                        rows="6"
                        placeholder="Tell me about your project..."
                    ></textarea>
                </div>

                <!-- Honeypot Spam Protection -->
                <input
                    type="checkbox"
                    name="botcheck"
                    class="hidden"
                    style="display: none"
                    tabindex="-1"
                />

                <!-- Success Message -->
                {#if submitSuccess}
                    <div class="success-message mb-6 p-4 bg-secondary/10 border border-secondary/30 rounded-default text-secondary">
                        <p class="font-medium">✓ Message sent successfully!</p>
                        <p class="text-sm mt-1">I'll get back to you within 24 hours.</p>
                    </div>
                {/if}

                <!-- Error Message -->
                {#if submitError}
                    <div class="error-message mb-6 p-4 bg-red-50 border border-red-200 rounded-default text-red-600">
                        <p class="font-medium">✗ Something went wrong</p>
                        <p class="text-sm mt-1">Please try again or email me directly.</p>
                    </div>
                {/if}

                <!-- Submit Button -->
                <button
                    type="submit"
                    disabled={isSubmitting}
                    class="w-full btn btn-primary cursor-pointer inline-flex items-center justify-center gap-2.5 px-7 py-4 rounded-default font-semibold text-[15px] transition-all duration-300 bg-gradient-to-b from-[#33363f] to-[#020202] text-white shadow-[0_0_0_1px_rgb(36,38,40),0_1px_2px_0_rgba(27,28,29,0.48)] hover:-translate-y-0.5 hover:shadow-[0_6px_20px_rgba(0,0,0,0.15)] disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:translate-y-0"
                >
                    {#if isSubmitting}
                        <span class="inline-block w-4 h-4 border-2 border-white border-t-transparent rounded-full animate-spin"></span>
                        Sending...
                    {:else}
                        Drop Me a Message
                    {/if}
                </button>
            </form>

            <!-- Contact Info -->
            <div class="contact-info mt-10 pt-10 border-t border-[rgba(17,16,17,0.1)]">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="info-item">
                        <h4 class="text-sm font-semibold text-text-light mb-1">Response Time</h4>
                        <p class="text-primary">Within 24 hours</p>
                    </div>
                    <div class="info-item">
                        <h4 class="text-sm font-semibold text-text-light mb-1">Availability</h4>
                        <p class="text-primary">Open for new projects</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>

<style>
    /* Mobile optimizations */
    @media (max-width: 768px) {
        .contact {
            padding: 60px 20px;
        }

        .section-header h2 {
            font-size: 32px;
        }

        .contact-card {
            padding: 30px 20px;
        }
    }

    /* Smooth animations */
    .success-message,
    .error-message {
        animation: slideDown 0.3s ease-out;
    }

    @keyframes slideDown {
        from {
            opacity: 0;
            transform: translateY(-10px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

    /* Loading spinner */
    @keyframes spin {
        to {
            transform: rotate(360deg);
        }
    }
</style>
