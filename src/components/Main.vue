<template>
  <v-container class="main">
    <section>
      <v-text-field label="Name" dense v-model="profile.contact_info.name" />
      <v-text-field label="Last" dense v-model="profile.contact_info.last_name" />
      <v-text-field label="EMail" dense v-model="profile.contact_info.email" />
      <v-text-field label="GitHub address" dense v-model="profile.contact_info.github" />
    </section>
    <ListManagement title="Educations" :names="['name', 'from', 'to']" :model="profile.educations"  v-slot:default="{add, item}">
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

    <ListManagement title="Skills" :names="['title']" :model="profile.skills"  v-slot:default="{item, add}">
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

    <ListManagement title="Open source projects" :names="['title', 'descript', 'url']" :model="profile.opensource_projects"  v-slot:default="{add, item}">
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
        <v-btn @click="save">Save</v-btn>
        <v-btn @click="load">Load</v-btn>
      </div>
    ==============
    <!-- <ListManagement :names="['name', 'from', 'to']" :model="educations">
    </ListManagement> -->

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
      input: Object,
      profile: {
        contact_info: {},
        educations: [{}],
        opensource_projects: [{}],
        skills: [{
          subs: [{}]
        }],
      }
    }
  },
  methods: {
    save() {
      var filename = this.profile.contact_info.name + '-' + this.profile.contact_info.last_name + ".json";
      var content = JSON.stringify(this.profile);
      var file = new Blob([content], {type: "text"});
      if (window.navigator.msSaveOrOpenBlob) // IE10+
          window.navigator.msSaveOrOpenBlob(file, filename);
      else { // Others
          var a = document.createElement("a"),
                  url = URL.createObjectURL(file);
          a.href = url;
          a.download = filename;
          document.body.appendChild(a);
          a.click();
          setTimeout(function() {
              document.body.removeChild(a);
              window.URL.revokeObjectURL(url);  
          }, 0); 
      }
    },
    load() {
      this.input.click();
    },
    store() {
      localStorage.profile = JSON.stringify(this.profile)
    },
    restore() {
      this.profile = JSON.parse(localStorage.profile || this.profile);
    }
  },
  mounted() {
    this.input = document.createElement("input");
    this.input.type = 'file';
    this.input.accept = "*.json";

    // var __reload = function(o){
    //     for (var k in o) {
    //         this.profile[k] = o[k];
    //     }
    // }
    var p = this.profile
    p;
    this.input.addEventListener("change", function(){
        var reader = new FileReader();
        reader.onload = function (e) {
            var contents = e.target.result;
            var obj = JSON.parse(contents);
            // this.profile = obj
            console.log("read", obj)
            // __reload(obj)
            p = obj;
        };
        reader.readAsText(this.files[0]);
    });

    this.profile = JSON.parse(localStorage.profile || this.profile);
  }
}
</script>
