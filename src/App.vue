<template>
    <div>
        <multiselect
            v-model="selectedValues"
            :items="options"
            :multiple="true"
            :tags="true"
            :object="true"
            label-prop="name"
            value-prop="id"
            :search="true"
            search-by="name"
            :hide-selected="true"
            :disabled="false"
            :default="defaultValues"
            label="Select Options"
            placeholder="Select options..."
            @change="handleChange"
            @select="handleSelect"
            @deselect="handleDeselect"
            @search-change="handleSearchChange"
        ></multiselect>
    </div>
</template>

<script>
import Multiselect from '@/components/MyMultiselect.vue';

export default {
    data() {
        return {
            options: [
                { name: '1' },
                { id: '2', name: 'Option 2' },
                { id: 2, name:  {id2: "2", name: 'O1'}},
                2,
                4,
                6,
                {},
                [],
            ],
            selectedValues: [1, 2, 'Option 1', name],
        };
    },
    components: {
        Multiselect
    },
    methods: {
        handleChange(newValue, oldValue) {
            console.log('Value changed:', newValue, oldValue);
        },
        handleSelect(option) {
            console.log('Option selected:', option);
        },
        handleDeselect(option) {
            console.log('Option deselected:', option);
        },
        handleSearchChange(query) {
            console.log('Search query changed:', query);
        }
    }
};
</script>

