<template>
    <ModalComponent v-show="show" @close="$emit('close')">
        <div class="flex flex-col">
            <h2 v-t="'actions.create_playlist'" />
            <input ref="input" v-model="playlistName" type="text" class="input mt-2" />
            <div class="ml-auto mt-3 w-min flex">
                <button v-t="'actions.cancel'" class="btn" @click="$emit('close')" />
                <button v-t="'actions.okay'" class="btn ml-2" @click="onCreatePlaylist" />
            </div>
        </div>
    </ModalComponent>
</template>

<script>
import ModalComponent from "./ModalComponent.vue";

export default {
    components: {
        ModalComponent,
    },
    props: {
        index: {
            type: Number,
            default: 0,
        },
    },
    emits: ["created", "close"],
    data() {
        return {
            playlistName: "New Playlist " + this.index,
            show: true,
        };
    },
    mounted() {
        this.$refs.input.focus();
        window.addEventListener("keydown", this.handleKeyDown);
    },
    unmounted() {
        window.removeEventListener("keydown", this.handleKeyDown);
    },
    methods: {
        handleKeyDown(event) {
            if (event.code === "Enter") {
                this.onCreatePlaylist();
                event.preventDefault();
            }
        },
        onCreatePlaylist() {
            if (!this.playlistName) return;

            this.createPlaylist(this.playlistName).then(response => {
                if (response.error) alert(response.error);
                else {
                    this.$emit("created", response.playlistId, this.playlistName);
                    this.$emit("close");
                }
            });
            this.show = false;
        },
    },
};
</script>
