<template>
    <select @change="onChange()" v-model="selected" class="form-select mt-10 block w-full border p-3 rounded" >
        <option value="0">Select a region</option>
        <option v-for="(reg, index) in region" :key="index" :value="index" >
            {{ reg.loc }}
        </option>
    </select>
</template>

<script>
export default {
    name: 'regionSelect',
    props: ['region'],
    data() {
        return {
            selected:0,
        }
    },
    methods:{
        onChange(){
            const ind = this.selected; 
            const reg = this.region[ind];
            this.$emit('get-region',reg)
        }
    },
}
</script>

