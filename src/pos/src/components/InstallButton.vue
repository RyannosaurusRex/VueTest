<template>
    <div
      class="btn btn-outline-success float-right"
      @click="installer()"
      :style="{'display' : installBtn}">
      {{ title }}
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class InstallButton extends Vue {
    @Prop() private title!: string;
    private installer: any = undefined;
    private installBtn: string = 'none';
    private created() {
        let installPrompt: any;

        window.addEventListener('beforeinstallprompt', (e) => {
            // Prevent Chrome 67 and earlier from automatically showing the prompt
            e.preventDefault();
            installPrompt = e;
            // Update UI/notify user they can add to home screen.
            this.installBtn = 'block';
        });

        this.installer = () => {
            this.installBtn = 'none';
            installPrompt.prompt();
            installPrompt.userChoice.then((result: any) => {
                if (result.outcome === 'accepted') {
                    //
                } else {
                    //
                }
                installPrompt = null;
            });
        };
    }
}
</script>