<template>
    <div class="icon-container" :class="{'copied': copied}" @click="copy(source.hex)">
        <span class="icon" v-html="'&#'+parseInt(source.hex,16)+';'"></span>
        <div class="name">{{source.name}}</div>
        <textarea ref="copybox" class="copybox"></textarea>
    </div>
</template>

<script>
    export default {
        name: 'item-component',
        props: {
            index: Number,
            source: {
                type: Object,
                default () {
                    return {}
                }
            }
        },

        data() {
            return {
                copied: false,
            }
        },

        methods: {
            copy(hex) {
                this.selected = hex
                this.copied = `&#${parseInt(hex, 16)};`

                this.$refs.copybox.value = this.copied
                this.$refs.copybox.select()

                document.execCommand('copy')
                
                setTimeout(() => {
                    this.copied = false
                }, 500)
            },
        },
    }
</script>

<style lang="sass" scoped>
    .icon-container
        height: 66px
        display: flex
        align-items: center
        user-select: none
        cursor: pointer
        border-bottom: 1px solid #101010
        color: var(--color)

        &.copied
            animation: 250ms fade forwards

        &:hover
            background: #dbedf320

        @keyframes fade
            00% 
                box-shadow: 0 0 0 0px #ffffff

            100% 
                box-shadow: 0 0 0 10px #ffffff00

        .icon
            font-family: var(--icon-font)
            display: grid
            place-content: center
            height: 100%
            width: 80px
            font-size: 35px
            font-weight: 300

        .name
            font-size: 15px

        .copybox
            position: absolute
            top: 0
            left: 0
            pointer-events: none
            opacity: 0
</style>