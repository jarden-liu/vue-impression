<template>
    <div class="input-text" v-disfavor="blur">
        <input
            type="tel"
            v-if="type === 'tel'"
            ref="input"
            v-model="currentValue"
            :disabled="currentDisabled"
            :placeholder="placeholder"
            @focus="focus = true"
            class="input-text-input">
        <input
            type="text"
            v-else
            ref="input"
            v-model="currentValue"
            :disabled="currentDisabled"
            :placeholder="placeholder"
            @focus="focus = true"
            class="input-text-input">
        <icon
            v-show="clearable && focus && !!currentValue"
            class="input-text-clear"
            name="times-circle"
            @click="clearHandle"
            size="lg" />
        <icon
            v-if="state"
            :name="getStateIcon()"
            :class="getStateClass()"
            size="lg" />
    </div>
</template>

<script>
    import Sync from '../mixins/sync';

    export default {
        name: 'input-text',
        mixins: [Sync],
        props: {
            clearable: Boolean,
            placeholder: String,
            type: {
                type: String,
                default: 'text',
                validator(value) {
                    return ['text', 'tel'].indexOf(value) > -1;
                },
            },
            state: {
                type: String,
                validator(value) {
                    return ['success', 'warning', 'error'].indexOf(value) > -1;
                },
            },
        },
        data() {
            return {
                focus: false,
                currentDisabled: this.disabled || this.$parent.disabled,
            };
        },
        methods: {
            blur() {
                this.focus = false;
            },
            clearHandle() {
                if(!this.currentDisabled) {
                    this.currentValue = '';
                    this.$refs.input.focus();
                }
            },
            getStateClass() {
                return {
                    success: 'text-success',
                    warning: 'text-warning',
                    error: 'text-danger',
                }[this.state];
            },
            getStateIcon() {
                return {
                    success: 'check',
                    warning: 'exclamation-triangle',
                    error: 'exclamation-circle',
                }[this.state];
            },
        },
    };
</script>
