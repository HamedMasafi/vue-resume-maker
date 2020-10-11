<template>
<v-expansion-panel>
    <v-expansion-panel-header>
        {{title}}
        <span v-if="model.length <= 1">
        </span>  
        <span v-if="model.length == 2">
            - One item
        </span>
        <span v-if="model.length > 2">
            - {{model.length - 1}} Items
        </span>
    </v-expansion-panel-header>
    <v-expansion-panel-content>

        <v-form ref="f" v-for="(item, i) in model" :key="i">
            <v-row class="item-row">
                <v-col cols="1" class="number-cell">
                    <span>#{{i + 1}}</span>
                    <v-btn @click="remove(i)" color="error" :disabled="i == model.length - 1" fab small>
                        <v-icon dark>
                            mdi-delete
                        </v-icon>
                    </v-btn>
                </v-col>
                <v-col cols="11">
                    <slot :add="add" :remove="remove" :item="item" :index="i">
                        <template v-for="(e, i) in names">
                            {{e}}-{{i}} <input v-model="item[e]" :key="i" @change="add" />
                        </template>
                    </slot>
                </v-col>
            </v-row>
        </v-form>
    </v-expansion-panel-content>

</v-expansion-panel>
</template>

<script>
export default {
    props: {
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
