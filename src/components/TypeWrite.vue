<template>
    <div>
        <p>{{ displayedText }}</p>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                text: 'Hello, World!',
                displayedText: ''
            };
        },
        mounted() {
            this.typingInterval = setInterval(() => {
                if (this.displayedText.length < this.text.length) {
                    this.displayedText += this.text.charAt(this.displayedText.length);
                } else {
                    clearInterval(this.typingInterval);
                }
            }, 100);
        },
        beforeDestroy() {
            clearInterval(this.typingInterval);
        }
    };
</script>

<style scoped>
    p {
        font-family: Arial, sans-serif;
        font-size: 18px;
        color: black;
    }
</style>