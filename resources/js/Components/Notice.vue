<template>
    <div
        class="notice"
        :class="cssClass"
        v-if="active && type && message"
    >
        <p
            class="notice__description"
            v-text="message"
        ></p>
    </div>
</template>

<script>
    import { router } from "@inertiajs/vue3";

    export default {
        data() {
            return {
                active: false,
                type: "",
                message: "",
                cssClass: "",
            };
        },

        mounted() {
            router.on('finish', () => {
                let message = router.page.props.message;
                let error = Object.values(router.page.props.errors)[0] || null;

                if (message) {
                    this.setNotice(router.page.props.message, "success");
                } else if (error) {
                    this.setNotice(error, "error");
                }
            });
        },

        methods: {
            setNotice(message, type) {
                this.type = type;
                this.message = message;

                this.setActive();
                this.setClasses();
            },

            setActive() {
                this.active = true;

                setTimeout(() => {
                    this.active = false;
                }, 3000);
            },

            setClasses() {
                if (this.type == "error") {
                    this.cssClass = "notice--error";
                } else if (this.type == "success") {
                    this.cssClass = "notice--success";
                }
            },
        },
    };
</script>

<style lang="scss">
    .notice {
        max-width: 400px;
        padding: 20px;
        position: fixed;
        right: 20px;
        bottom: 20px;
        z-index: 950;
        border-radius: radius(1);

        &--error {
            background-color: color(error-bg);
            // Type
            color: color(error-text);
        }

        &--success {
            background-color: color(success-bg);
            // Type
            color: color(success-text);
        }
    }

    .notice__description {
        @include rem(16px);
    }
</style>
