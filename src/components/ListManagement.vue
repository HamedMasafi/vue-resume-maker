<template>
<section>
    <div v-for="(item, i) in model" :key="i">
        <!-- <slot name="items"  :item="item"></slot> -->
        <slot :add="add" :remove="remove" :item="item" :index="i">
            <template v-for="(e, i) in names" >
                {{e}}-{{i}} <input v-model="item[e]" :key="i" @change="add"/>
            </template>
        </slot>
    </div>
</section>
</template>
<script>
export default {
    props:{
        model: Array,
        names: Array
    },
    methods: {
        add() {
            var e = this.model[this.model.length - 1];
            for (var p in this.names) {
                var v = e[this.names[p]]
                if (v === '' || v === null || v === undefined) {
                    return;
                }
                console.log(p)
            }
            this.model.push({})
        },
        remove(index) {
            this.model.splice(index, 1)
        }
    }
}
</script>