<template>
    <div>
        <multiselect
            v-model="selectedValues"
            :value="selectedValues"
            :items="options"
            :multiple="true"
            :tags="true"
            :object="true"
            label-prop="name"
            value-prop="id"
            :hide-selected="true"
            :disabled="false"
            label="Select Options"
            @change="handleChange"
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
                "424124ss",
                2,
                4,
                6,
                { name: 'Vue.js', code: 'vu' },
                {},
                [],
            ],
            selectedValues: [{ name: '1' },
                { id: '2', name: 'Option 2' },
                { id: 2, name:  {id2: "2", name: 'O1'}},
                2,
                4,
                6,
                { name: 'Vue.js', code: 'vu' },
                {},
                [],
            ],

        };
    },
    components: {
        Multiselect
    },
};
</script>

