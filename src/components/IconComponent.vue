<script setup>
import { defineProps, computed } from 'vue';

const props = defineProps({
    icon: {
        type: String,
        required: true,
    },
    active: {
        type: Boolean,
        default: false
    }
});

const iconDescription = computed(() => props.icon)
const isActive = computed(() => props.active)
</script>

<template>
    <span :class="['material-symbols-outlined', { 'active-icon': isActive }]">
        {{ iconDescription }}
    </span>
</template>

<style scoped>
.material-symbols-outlined {
    font-variation-settings:
        'FILL' 0,
        'wght' 400,
        'GRAD' 0,
        'opsz' 48;

    width: 40px;
    height: 40px;
    border-radius: 50%;
    background-color: #202325;
    display: flex;
    justify-content: center;
    align-items: center;
}

.active-icon {
    color: #1ca060;
}
</style>