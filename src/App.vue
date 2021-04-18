<template>
    <div id="app">
        <div class="menu">
            <div class="tab-container">
                <button v-for="(iSet, id) in iconsets" :key="id" @click="setIconset(id)" :class="{'active': id === iconset}">
                    <span class="name">{{iSet.name}}</span>
                    <span class="version">{{iSet.version}}</span>
                </button>
            </div>

            <input onclick="this.select()" v-on:input="searchIcons()" type="text" placeholder="Search" autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false" class="search" v-model="search">
        </div>
        <div class="list">
            <virtual-list ref="list" style="height: 100vh; overflow-y: auto;" :data-key="'hex'" :data-sources="searchedIcons" :data-component="iconComponent"/>
        </div>
    </div>
</template>

<script>
    import VirtualList from 'vue-virtual-scroll-list'
    import IconComponent from './components/IconComponent.vue'

    import MDI_3_4_93 from './assets/js/mdi_3_4_93.json'
    import MDI_4 from './assets/js/mdi_4.json'

    export default {
        data() {
            return {
                iconComponent: IconComponent,
                
                search: '',
                searchedIcons: [],

                iconset: '',
                iconsets: {
                    'mdi_4':      {name: 'Material Design Icons', version: '4.0.0', fontName: 'MDI4', path: './assets/downloads/MDI_4.zip', list: MDI_4},
                    'mdi_3_4_93': {name: 'Material Design Icons', version: '3.4.93', fontName: 'MDI3', path: './assets/downloads/MDI_3_4_93.zip', list: MDI_3_4_93},
                },
            }
        },

        mounted() {
            this.searchIcons()
            this.setIconset('mdi_4')
        },

        computed: {
            selectedIconset() {
                if (!this.iconsets.hasOwnProperty(this.iconset))
                {
                    return {
                        name: 'unknown',
                        version: null,
                        path: null,
                        list: [],
                    }
                }

                return this.iconsets[this.iconset]
            },
        },

        methods: {
            setIconset(iconsetId) {
                this.iconset = iconsetId
                document.documentElement.style.setProperty('--icon-font', this.iconsets[this.iconset].fontName)
                this.searchIcons()
            },

            searchIcons() {
                this.searchedIcons = []

                let cleanSearch
                cleanSearch = this.search.toUpperCase()
                cleanSearch = cleanSearch.replace(/[^A-Z0-9]/g,'')
                
                for (const icon of this.selectedIconset.list)
                {
                    let cleanName
                    cleanName = icon.name.toUpperCase()
                    cleanName = cleanName.replace(/[^A-Z0-9]/g,'')

                    if (cleanName.includes(cleanSearch))
                    {
                        this.searchedIcons.push(icon)
                    }
                }
            },
        },

        components: {
            'virtual-list': VirtualList
        }
    }
</script>

<style lang="sass">
    @import url('./assets/sass/fonts.css')
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&family=Roboto:wght@400;700&display=swap')

    *
        box-sizing: border-box

    *:focus
        outline: none

    html, body
        margin: 0
        padding: 0
        height: 100%
        font-family: 'Roboto', sans-serif
        background: var(--bg)
        color: var()

    :root
        --icon-font: 'MDI4'
        --bg: #393e46
        --color: #dbedf3

    #app
        height: 100%
        max-height: 100%
        display: grid
        grid-template-rows: 1fr
        grid-template-columns: 400px 1fr
        grid-template-areas: "menu list"

        .menu
            grid-area: menu
            background: var(--bg)
            border-right: 1px solid #101010
            filter: drop-shadow(0 3px 5px #00000060)

            .tab-container
                display: flex
                gap: 10px
                border-bottom: 1px solid #101010
                padding: 10px

                button
                    flex: 1
                    background: transparent
                    border: none
                    display: flex
                    flex-direction: column
                    color: var(--color)
                    cursor: pointer
                    padding: 10px 0
                    align-items: center
                    border-radius: 7px
                    transition: background 100ms

                    &:hover
                        background: #ffffff0a

                    &:focus
                        box-shadow: 0 0 0 3px #ffffff0a

                    .name
                        line-height: 150%
                        letter-spacing: 0.5px
                        font-size: 12px

                    .version
                        line-height: 150%
                        color: #ffffff90
                        letter-spacing: 1px
                        font-size: 12px

                    &.active
                        background: #ffffff10

            .search
                display: block
                width: calc(100% - 20px)
                height: 44px
                border-radius: 50px
                padding: 0 20px
                color: var(--color)
                background: var(--bg)
                border: 1px solid #101010
                font-size: 15px
                font-family: 'Roboto'
                letter-spacing: 1px
                margin: 10px

                &::placeholder
                    color: #ffffff60

        .list
            grid-area: list
            height: 100%
            max-height: 100%
</style>
