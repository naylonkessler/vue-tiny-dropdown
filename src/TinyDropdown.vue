<template>
    <div class="dropdown">
        <slot
            name="trigger"
            :label="label"
            :show-arrow="showArrow"
            :toggle="toggle"
            :value="value"
        >
            <button
                @click.prevent="toggle"
                class="dropdown__trigger"
                type="button"
            >
                <TinyOptionContent
                    v-if="value"
                    :option="value"
                />
                <span v-else>{{ label }}</span>
                <i
                    v-if="showArrow"
                    class="dropdown__trigger-icon"
                ></i>
            </button>
        </slot>
        <nav
            v-if="isOpen"
            class="dropdown__panel"
        >
            <slot
                name="options"
                :select="select"
                :nullable="nullable"
                :null-option-label="nullOptionLabel"
            >
                <a
                    @click.prevent="select(null)"
                    v-if="nullable"
                    class="dropdown__option"
                    href="#"
                >
                    <TinyOptionContent :option="nullOptionLabel" />
                </a>
                <template v-for="option of options">
                    <a
                        v-if="!isDivider(option)"
                        @click.prevent="select(option)"
                        class="dropdown__option"
                        href="#"
                    >
                        <TinyOptionContent :option="option" />
                    </a>
                    <TinyOptionsDivider v-else />
                </template>
            </slot>
        </nav>
    </div>
</template>

<script>
    import TinyOptionContent from './OptionContent'
    import TinyOptionsDivider from './OptionsDivider'

    export default {
        components: {
            TinyOptionContent,
            TinyOptionsDivider
        },

        props: {
            label: {
                default: 'Choose a value ...',
                type: String
            },
            nullable: {
                default: false,
                type: Boolean
            },
            nullOptionLabel: {
                default: 'None value',
                type: String
            },
            options: Array,
            showArrow: {
                default: true,
                type: Boolean
            },
            value: [Object, Number, String]
        },

        data () {
            return {
                isOpen: false
            }
        },

        created () {
            this.bindDocumentEvents()
        },

        destroyed () {
            this.unbindDocumentEvents()
        },

        methods: {
            bindDocumentEvents () {
                document.addEventListener('click', this.documentClick)
            },

            documentClick (eve) {
                const el = this.$el

                if (el !== eve.target && !el.contains(eve.target)) {
                    this.isOpen = false
                }
            },

            isDivider (option) {
                return option === '-' || option.label === '-'
            },

            select (option) {
                this.toggle()
                this.$emit('selected', option)
                this.$emit('input', option)
            },

            toggle () {
                this.isOpen = !this.isOpen
            },

            unbindDocumentEvents () {
                document.removeEventListener('click', this.documentClick)
            }
        }
    }
</script>
