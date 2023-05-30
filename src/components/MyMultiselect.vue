<template>
    <h1>Chips</h1>
    <h4>Use chips prop to make selected option as chip.</h4>
    <div class="multiselect"
         :style="{ width: width }"
         @blur="focused = false"
         ref="parent"
         @click="handleClick">

        <label v-if="label">{{ label }}</label>

        <div class="multiselect-wrapper"
             :class="{ 'disabled': disabled }">

            <div class="selected-tags">
        <span
                class="tag"
                v-for="selectedOption in selectedOptions"
                :key="getOptionValue(selectedOption)"
                @click="deselectOption(selectedOption)">

            {{ getOptionLabel(selectedOption) }}

            <i class="close-icon"></i>
        </span>
            </div>

            <ul class="option-list"
                :class="{ 'hide-selected': hideSelected, 'show': showOptions }"
                :style="{ top: optionsTop }"
                v-show="focused">
                <li
                        v-for="option in filteredOptions"
                        :key="getOptionValue(option)"
                        @click="toggleOption(option)"
                        :class="{ selected: isOptionSelected(option)}"
                >
                    {{ getOptionLabel(option) }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import {vShow} from "vue";

export default {
    data() {
        return {
            focused: false,
            optionsTop: "34px",
            showCount: true,
            selectedOptions: [],
            searchQuery: '',
            showOptions: false,
            hideSelected: false
        };
    },

    props: {
        value: {
            type: [Object, Array, String, Number, null],
            default: null
        },
        items: {
            type: [Object, Array],
            default: () => ({})
        },
        multiple: {
            type: Boolean,
            default: true
        },
        tags: {
            type: Boolean,
            default: true
        },
        width: {
            type: String,
            default: "325px",
        },
        object: {
            type: Boolean,
            default: true
        },
        labelProp: {
            type: String,
            default: 'label'
        },
        valueProp: {
            type: String,
            default: 'id'
        },
        hideSelected: {
            type: Boolean,
            default: false
        },
        disabled: {
            type: Boolean,
            default: false
        },
        default: {
            type: Array,
            default: () => []
        },
        label: {
            type: String,
            default: null
        },
        placeholder: {
            type: String,
            default: null
        }
    },

    computed: {


        filteredOptions() {
            if (this.search && this.searchQuery) {
                const query = this.searchQuery.toLowerCase();
                return this.getOptions().filter(option =>
                    this.getOptionLabel(option).toLowerCase().includes(query)
                );
            } else {
                return this.getOptions();
            }
        }
    },

    watch: {
        value: {
            immediate: true,
            handler(newValue, oldValue) {
                if (newValue !== oldValue) {
                    this.selectedOptions = this.getSelectedOptions();
                    this.$emit('change', newValue, oldValue);
                }
            }
        }
    },

    mounted() {
        this.fixTop();
    },

    methods: {
        fixTop() {
            this.optionsTop = this.$refs.parent.clientHeight + 2 + "px";
        },
        handleClick(){
            this.focused = !this.focused;
        },
        toggleOption(option) {
            if (this.isOptionSelected(option)) {
                this.deselectOption(option);
            } else {
                this.selectOption(option);
            }
        },

        getOptions() {
            if (Array.isArray(this.items)) {
                return this.items;
            } else if (typeof this.items === 'object') {
                return Object.values(this.items);
            } else {
                return [];
            }
        },

        getOptionLabel(option) {
            if (typeof option === 'object' && option !== null) {
                return option[this.labelProp] || '';
            } else {
                return option;
            }
        },

        getOptionValue(option) {
            if (typeof option === 'object' && option !== null) {
                return option[this.valueProp] || '';
            } else {
                return option;
            }
        },

        isOptionSelected(option) {
            if (this.multiple) {
                return this.selectedOptions.some(selectedOption =>
                    this.getOptionValue(selectedOption) === this.getOptionValue(option)
                );
            } else {
                return this.getOptionValue(this.selectedOptions) === this.getOptionValue(option);
            }
        },

        getSelectedOptions() {
            if (this.multiple) {
                return this.value || [];
            } else {
                return this.value ? [this.value] : [];
            }
        },

        selectOption(option) {
            if (!this.disabled) {
                if (this.multiple) {
                    const selectedOptions = [...this.selectedOptions];
                    if (!this.isOptionSelected(option)) {
                        selectedOptions.push(option);
                        this.selectedOptions = selectedOptions;
                        this.$emit('select', option);
                    }
                } else {
                    this.selectedOptions = [option];
                    this.$emit('select', option);

                }
            }
            setTimeout(this.fixTop, 100);
        },

        deselectOption(option) {
            if (!this.disabled && this.isOptionSelected(option)) {
                let selectedOptions = this.selectedOptions.filter(selectedOption =>
                    this.getOptionValue(selectedOption) !== this.getOptionValue(option)
                );
                this.selectedOptions = selectedOptions;
                this.$emit('deselect', option);
                this.emitValue();
            }
            setTimeout(this.fixTop, 100);
        },

        emitValue() {
            if (this.multiple) {
                const values = this.selectedOptions.map(option => this.getOptionValue(option));
                this.$emit('input', values);
            } else {
                const selectedOption = this.selectedOptions.length ? this.selectedOptions[0] : null;
                this.$emit('input', this.getOptionValue(selectedOption));
            }
        }
    },
};
</script>

<style scoped>
body {
    background-color: #f4f5fb;
}

.input-with-arrow {
    position: relative;
}

.input-with-arrow .arrow {
    position: absolute;
    top: 15px;
    left: 20px;
    transform: translateY(-50%);
    cursor: pointer;
    transition: transform 0.3s ease;
    color: #dcdbdd;
}

.input-with-arrow .arrow svg {
    fill: #aaa;
    width: 550px;
    height: 20px;
}

.input-with-arrow .arrow.rotated svg {
    transform: rotate(180deg);
    transition: transform 0.3s ease;
}

.background-card {
    padding: 10px 0px 40px 30px;
    width: 400px;
    left: 320px;
    z-index: -9999;
    background-color: #fff;
    border-radius: 6px;
    box-shadow: 0 4px 5px -2px;
}

.multiselect {
    padding: 6px 12px;
    margin: 8px 0;
    display: inline-block;
    border-radius: 4px;
    box-sizing: border-box;
    min-height: 33px;
    min-width: 222px;
    position: relative;
    display: flex;
    flex-wrap: wrap;
    outline: 1px solid #d2d1d3;
}

.multiselect:hover {
    outline: 1px solid #97969b;
    transition: .2s linear;
}

.multiselect:focus {
    outline: 2px solid #9156fd;
}

.hide-selected {
    display: none;
    color: #929292;
}

.tag {
    padding: 4px 8px;
    padding-right: 0;
    margin: 3px 3px;
    border-radius: 99999px;
    background-color: #f3f3f4;
    color: #939198;
    z-index: 2;
}

.close-icon {
    cursor: pointer;
    padding-right: 7px;
}

.close-icon:hover {
    color: red;
    font-weight: bold;
}

.option-list {
    display: flex;
    position: absolute;
    right: 0;
    left: 0;
    background: #fff;
    flex-direction: column;
    box-shadow: 0 0 3px 3px #ececed;
    padding: 05px 0;
    min-height: 55px;
    max-height: 188px;
    overflow-y: auto;
    border-radius: 3px;
}

.option-list {
    padding: 6px 11px;
    cursor: pointer;
    display: flex;
    align-items: center;
}

.my-multiselect__optiomultiselect-wrappern--checked {
    color: #504b56;
    font-weight: bold;
}

.my-multiselect__option--checked:hover {
    background-color: #eeeeef;
    font-weight: bold;
}

.multiselect__text:hover {
    background-color: #eeeeef;
    font-weight: bold;
}

.multiselect__text::after {
    background-color: #eeeeef;
    font-weight: bold;
}

.multiselect__option:hover {
    background-color: #eeeeef;
}

.multiselect__checkbox {
    width: 22px;
    height: 22px;
    border: 2px solid #969696;
    margin-right: 7px;
    position: relative;
    /* background: #1f7bb8; */
    border-radius: 3px;
}

.multiselect__option--checked .multiselect__checkbox {
    background: #504b56;
    border: none;
    border-radius: 3px;
}

.multiselect__option--checked .multiselect__checkbox::after {
    width: 14px;
    height: 9px;
    border-left: 2px solid rgb(255, 255, 255);
    border-bottom: 2px solid rgb(255, 255, 255);
    content: "";
    z-index: 9999;
    position: absolute;
    transform: rotate(-45deg);
    left: 4px;
    top: 4px;
}
</style>
