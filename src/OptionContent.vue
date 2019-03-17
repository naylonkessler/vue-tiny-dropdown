<template>
    <span class="dropdown-option-content">
        <i
            v-if="option.icon"
            class="material-icons dropdown-option-content__icon"
        >{{ option.icon }}</i>
        <img
            v-if="option.image"
            :src="option.image"
            class="dropdown-option-content__image"
        />
        <span class="dropdown-option-content__label">{{ option.label || option }}</span>
    </span>
</template>

<script>
    export default {
        props: {
            option: [Object, Number, String]
        }
    }
</script>
