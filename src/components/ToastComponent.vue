<template>
    <div class="toast">
        <slot />
        <button v-t="'actions.dismiss'" @click="dismiss" />
    </div>
</template>

<script>
import hotkeys from "hotkeys-js";
export default {
    emits: ["dismissed"],
    mounted() {
        hotkeys("space", () => {
            this.dismiss();
        });
    },
    methods: {
        dismiss() {
            this.$emit("dismissed");
        },
    },
};
</script>

<style>
.toast {
    @apply bg-white/80 text-black flex flex-col justify-center fixed top-12 right-12 p-4 min-w-max shadow rounded duration-200 z-9999;
}
.dark .toast {
    @apply bg-dark-900/80 text-white;
}
.toast button {
    @apply underline;
}
</style>
