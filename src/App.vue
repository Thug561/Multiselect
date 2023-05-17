<template>
    <div class="switch">
        <div class="demo">
            <div class="demo__left">
                <my-multiselect
                    :options="values"
                    display-property="title"
                    value-property="shortcut"
                    v-model="selectedValue"
                    :show-count="showCount"
                ></my-multiselect>
            </div>
        </div>
    </div>
</template>

<script>
import MyMultiselect from "./components/MyMultiselect.vue";

export default {
    data() {
        return {
            values: [
                { title: "bar", shortcut: "value1" },
                { title: "foo", shortcut: "value2" },
                { title: "fizz", shortcut: "value3" },
                { title: "buzz", shortcut: "value4" },
                { title: "foo2", shortcut: "value5" },
                { title: "bar2", shortcut: "value6" },
                { title: "fizz2", shortcut: "value7" },
            ],
            selectedValue: ["value1", "value2", "value3", "value4"]
        };
    },
    components: {
        MyMultiselect,
    },
    computed: {
        filteredValues() {
            const selected = this.selectedValue;
            const filtered = this.values.filter((value) =>
                selected.includes(value.shortcut)
            );
            return filtered.slice(0, 4);
        },
        remainingCount() {
            const selected = this.selectedValue;
            const filtered = this.values.filter((value) =>
                selected.includes(value.shortcut)
            );
            return Math.max(filtered.length - 4, 0);
        },
        shouldShowCount() {
            return this.selectedValue.length > 4;
        },
        placeholderText() {
            if (this.shouldShowCount) {
                return `${this.selectedValue.length} selected`;
            } else {
                return this.selectedValue
                    .map((shortcut) =>
                        this.values.find((value) => value.shortcut === shortcut)
                    )
                    .map((value) => value.title)
                    .join(", ");
            }
        },
    },
};
</script>
