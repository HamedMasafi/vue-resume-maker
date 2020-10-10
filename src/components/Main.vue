<template>
  <div class="hello">
    <ListManagement :names="['name', 'from', 'to']" :model="educations"  v-slot:default="{add, remove, item, index}">
        <div slot="items">
           Name: <input v-model="item.name" @change="add"/> 
        <input v-model="item.from" @change="add"/> 
        <input v-model="item.to" @change="add"/> 

        <button @click="remove(index)">x</button>
        </div>
    </ListManagement>
    ==============
    <ListManagement :names="['name', 'from', 'to']" :model="educations">
    </ListManagement>
  </div>
</template>

<script>
import ListManagement from './ListManagement.vue'

export default {
  name: 'Main',
  components: {
      ListManagement
  },
  data () {
    return {
    drawer: null,
    educations: [{}]
    }
  }
}
</script>
