<template>
    <div>
        <MyMultiselect
                :options="options"
                v-model="selectedOptions"
                placeholder="Select options"
                displayProperty="name"
                valueProperty="id"
        />
    </div>
</template>

<script>
import MyMultiselect from './components/MyMultiselect.vue';

export default {
    components: {
        MyMultiselect,
    },
    data() {
        return {
            options: [
                { id: 1, name: 'Option 1' },
                { id: 2, name: 'Option 2' },
                { id: 3, name: 'Option 3' },
            ],
            selectedOptions: [],
        };
    },
};
</script>