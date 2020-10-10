<template>
<v-card elevation="10">
            <v-card-text>

    <h1>{{title}}</h1>
    <v-row v-for="(item, i) in model" :key="i">
        <!-- <slot name="items"  :item="item"></slot> -->
        <v-col cols="1" class="number-cell">
        <span>{{i + 1}}</span>
        </v-col>
        <v-col cols="10">
        <slot :add="add" :remove="remove" :item="item" :index="i">
            <template v-for="(e, i) in names" >
                {{e}}-{{i}} <input v-model="item[e]" :key="i" @change="add"/>
            </template>
        </slot>
        </v-col>
        <v-col cols="1">
        <v-btn @click="remove(i)" color="error">
            <v-icon dark>
                mdi-delete
            </v-icon>
        </v-btn>
        </v-col>
    </v-row>
        </v-card-text>

</v-card>
</template>
<script>
export default {
    props:{
        title: String,
        model: Array,
        names: Array
    },
    methods: {
        add() {
            var e = this.model[this.model.length - 1];
            for (var p in this.names) {
                var v = e[this.names[p]]
                if (v === '' || v === null || v === undefined) {
                    console.log('prop is null', this.names[p])
                    return;
                }
                console.log(p)
            }
            this.model.push({})
        },
        remove(index) {
            var p = confirm("Are you sure to delete this item?") 
            if (this.model.length > 1 && p)
                this.model.splice(index, 1)
        }
    }
}
</script>