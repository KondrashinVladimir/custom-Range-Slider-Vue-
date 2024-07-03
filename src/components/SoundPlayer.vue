<template>
    <div>
        <button @click="playSound">Воспроизвести звук</button>
    </div>
</template>

<script>
import { Howl } from 'howler';

export default {
    data() {
        return {
            sound: null
        };
    },
    mounted() {
        this.sound = new Howl({
            src: ['../assets/sound.wav']
        });
    },
    methods: {
        playSound() {
            if (this.sound) {
                const soundPromise = this.sound.play();

                if (soundPromise !== undefined) {
                    soundPromise.then(() => {
                    }).catch((error) => {
                        console.error('Ошибка воспроизведения звука:', error);
                    });
                }
            }
        }
    }
};
</script>