<template>
    <div class="autocomplete" ref="autocomplete">
        <input
            :name="name"
            type="text"
            ref="input"
            class="autocomplete-input"
            v-model="query"
            @keydown.up="up"
            @keydown.down="down"
            @keydown.enter="selectItem"
            @keydown.esc="focusout"
        >

        <ul class="autocomplete-options" v-show="showList" ref="itemsList">
            <li
                v-for="(match, index) in matches"
                :key="match.id"
                :class="{ selected: (selectedIndex === index) }"
                @click="clickedItem(index)"
            >
                {{ match[filterBy] }}
            </li>
        </ul>

    </div>
</template>

<script>
export default {
    props: {
        minQuerySize: {
            type: [String, Number],
            default: 1
        },
        items: {
            type: Array,
            default: [],
        },
        filterBy: String,
        name: String,
    },
    mounted() {
        document.querySelectorAll(':not(.autocomplete-option)').forEach(element => {
            element.addEventListener('click', this.focusout)
        })
    },
    data() {
        return {
            showList: true,
            query: '',
            selectedIndex: 0,
            itemHeight: 40,
            selectedItem: null,
        }
    },
    methods: {
        clickedItem(index) {
            this.selectedIndex = index
            this.selectItem()
        },
        selectItem() {
            if (!this.matches.length) {
                return
            }

            this.selectedItem = this.matches[this.selectedIndex]
            this.showList = false

            this.$refs.input.value = this.selectedItem[this.filterBy]
            this.$refs.input.focus()
            this.selectedIndex = 0
            this.query = this.selectedItem[this.filterBy]

            this.$emit('selected', JSON.parse(JSON.stringify(this.selectedItem)))
        },
        up() {
            if (this.selectedIndex === 0) {
                return
            }

            this.selectedIndex--
            this.scrollToItem()
        },
        down() {
            if (this.selectedIndex === this.matches.length - 1) {
                return
            }

            this.selectedIndex++

            this.scrollToItem()
        },
        scrollToItem() {
            this.$refs.itemsList.scrollTop = this.selectedIndex * this.itemHeight
        },
        focusout() {
            this.showList = false
        }
    },
    computed: {
        matches() {
            if (this.query.length < this.minQuerySize) {
                return []
            }

            return this.items.filter(item => (
                item[this.filterBy].toLowerCase().includes(this.query.toLowerCase())
            ))
        }
    },
    watch: {
        query(value) {
            if (this.selectedItem !== null) {
                return this.selectedItem = null
            }

            return this.showList = true
        }
    }
}
</script>

<style scoped>
    .autocomplete {
        width: 100%;
        font-family: Roboto,-apple-system,BlinkMacSystemFont,'Helvetica Neue',Helvetica,sans-serif;
        position: relative;
    }

    input {
        width: 100%;
        height: 40px;
        border: 1px solid #ccc;
        border-radius: 3px;
        padding: 5px 15px;
        font-size: 18px;
        color: #333;
    }

    ul {
        margin-top: 10px;
        max-height: 200px;
        overflow-y: scroll;
        font-weight: bold;
        position: absolute;
        left: 0;
        right: 0;
    }

    ul::-webkit-scrollbar {
        width: 5px;
        background: transparent;
    }

    ul::-webkit-scrollbar-thumb {
        background: #777;
    }

    li {
        list-style: none;
        height: 40px;
        background: #FFF;
        padding: 10px 15px;
        border-left: 1px solid #ccc;
        border-right: 1px solid #ccc;
        cursor: pointer;
    }

    li:hover {
        background: #7ea5e9;
        color: #FFF;
    }

    li.selected {
        background: #4C8BF5;
        color: #FFF;
    }

    li:first-child {
        border-top-left-radius: 3px;
        border-top-right-radius: 3px;
        border-left: 1px solid #ccc;
        border-right: 1px solid #ccc;
        border-top: 1px solid #ccc;
    }

    li:last-child {
        border-bottom-left-radius: 3px;
        border-bottom-right-radius: 3px;
        border-left: 1px solid #ccc;
        border-right: 1px solid #ccc;
        border-bottom: 1px solid #ccc;
    }
</style>
