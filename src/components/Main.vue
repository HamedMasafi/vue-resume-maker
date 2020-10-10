<template>
  <v-container class="main">
    <section>
      <v-text-field label="Name" dense v-model="contact_info.name" />
      <v-text-field label="Last" dense v-model="contact_info.last_name" />
      <v-text-field label="EMail" dense v-model="contact_info.last_name" />
      <v-text-field label="GitHub address" dense v-model="contact_info.last_name" />
    </section>
    <ListManagement title="Educations" :names="['name', 'from', 'to']" :model="educations"  v-slot:default="{add, item}">
        <v-row slot="items">
          <v-col>
            <v-text-field dense v-model="item.name" @change="add"/> 
          </v-col>
          <v-col>
            <v-text-field dense v-model="item.from" @change="add"/> 
          </v-col>
          <v-col>
            <v-text-field dense v-model="item.to" @change="add"/> 
          </v-col>
        </v-row>
    </ListManagement>

    <!-- <ListManagement title="skills" :names="['title', 'skill', 'years']" :model="skills"  v-slot:default="{add, item}">
        <div slot="items"> 
          <v-text-field dense v-model="item.title" @change="add"/> 
          <select v-model="item.skill" @change="add"> 
            <option>Beginner</option>
            <option>Midlevel</option>
            <option>Junior</option>
          </select>
          <v-text-field dense type="number" v-model="item.years" @change="add"/> 
        </div>
    </ListManagement> -->

    <ListManagement title="Skills" :names="['title']" :model="skills"  v-slot:default="{item, add}">
        <v-text-field label="Skill group name" dense v-model="item.title" @change="add"/> 
        <ListManagement :title="item.title" :names="['title', 'skill', 'years']" :model="item.subs"  v-slot:default="{add, item}">
          <v-row slot="items">
            <v-col>
              <v-text-field label="Title" dense v-model="item.title" @change="add"/> 
            </v-col>
            <v-col>
              <v-select dense v-model="item.skill" @change="add" :items="['Beginner', 'Advanced']"> 
              </v-select>
            </v-col>
            <v-col>
              <v-text-field label="Years" dense type="number" v-model="item.years" @change="add"/> 
            </v-col>
          </v-row>
      </ListManagement>
    </ListManagement>

    <ListManagement title="Open source projects" :names="['title', 'descript', 'url']" :model="opensource_projects"  v-slot:default="{add, item}">
          <v-row slot="items">
            
            <v-col>
              <v-text-field dense label="Title" v-model="item.title" @change="add"/> 
            </v-col>
            <v-col>
              <v-text-field dense label="Descript" v-model="item.descript" @change="add"/> 
            </v-col>
            <v-col>
              <v-text-field dense label="Url" v-model="item.url" @change="add"/> 
            </v-col>
          </v-row>
      </ListManagement>

      <div>
        <v-btn>Save</v-btn>
      </div>
    ==============
    <!-- <ListManagement :names="['name', 'from', 'to']" :model="educations">
    </ListManagement> -->
    {{skills}}
  </v-container>
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
    contact_info: {},
    educations: [{}],
    opensource_projects: [{}],
    skills: [{
      subs: [{}]
    }],
    }
  }
}
</script>
