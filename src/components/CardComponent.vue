<script setup>
import { ref, defineProps, computed } from 'vue';

const props = defineProps({
    color: {
        type: String,
        default: 'info'
    }
});

const cardClass = computed(() => `card-color--${props.color}`);

</script>

<template>
    <div :class="['card-component', cardClass]">
        <slot></slot>
    </div>
</template>

<style scoped>
.card-component {
    width: 400px;
    height: 500px;
    background-color: #212227;
    border-radius: 5%;
    padding: 30px;
}

.card-color--info {
    border-bottom: 5px solid #0e6ec2;
    border-top: 5px solid #0e6ec2;
}

.card-color--warning {
    border-bottom: 5px solid #ea6809;
    border-top: 5px solid #ea6809;
}

.card-color--success {
    border-bottom: 5px solid #4be314;
    border-top: 5px solid #4be314;
}

.card-color--error {
    border-bottom: 5px solid #b31010;
    border-top: 5px solid #b31010;
}

.card-color--default {
    border-bottom: 5px solid #3D3E49;
    border-top: 5px solid #3D3E49;
}
</style>
