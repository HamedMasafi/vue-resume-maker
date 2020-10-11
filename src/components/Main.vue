<template>
<v-container class="main">
    <v-expansion-panels>
        <v-expansion-panel>
            <v-expansion-panel-header>
              Personal Info
            </v-expansion-panel-header>
            <v-expansion-panel-content>
                <v-text-field label="Name" dense v-model="profile.personal_info.name" />
                <v-text-field label="Last" dense v-model="profile.personal_info.last_name" />
                <v-text-field label="EMail" dense v-model="profile.personal_info.email" />
                <v-text-field label="GitHub address" dense v-model="profile.personal_info.github" />
            </v-expansion-panel-content>
        </v-expansion-panel>

        <ListManagement title="Contact info" :names="['type', 'value']" :model="profile.contact_info" v-slot:default="{add, item}">
            <v-row slot="items">
                <v-col>
                    <v-select dense v-model="item.type" @change="add" :items="contact_types">
                    </v-select>
                </v-col>
                <v-col>
                    <v-text-field label="Value" dense v-model="item.value" @change="add" />
                </v-col>
            </v-row>
        </ListManagement>

        <ListManagement title="Educations" :names="['name', 'from', 'to']" :model="profile.educations" v-slot:default="{add, item}">
            <v-row slot="items">
                <v-col>
                    <v-text-field label="Title" dense v-model="item.name" @change="add" />
                </v-col>
                <v-col>
                    <v-text-field label="From date" dense v-model="item.from" @change="add" :rules="[rules.date]" />
                </v-col>
                <v-col>
                    <v-text-field label="To date" dense v-model="item.to" @change="add" :rules="[rules.date]" />
                </v-col>
            </v-row>
        </ListManagement>

        <ListManagement title="Jobs" :names="['name', 'position', 'from', 'to', 'since_now']" :model="profile.jobs" v-slot:default="{add, item}">
            <v-row slot="items">
                <v-col>
                    <v-text-field label="Title" dense v-model="item.name" @change="add" />
                </v-col>
                <v-col>
                    <v-text-field label="Position" dense v-model="item.position" @change="add" />
                </v-col>
                <v-col>
                    <v-text-field label="From date" dense v-model="item.from" @change="add" :rules="[rules.date]" />
                </v-col>
                <v-col>
                    <v-text-field label="To date" dense v-model="item.to" :disabled="item.since_now" @change="add" :rules="[rules.date]" />
                </v-col>
                <v-col cols="12">
                    <v-checkbox label="I'm working here now" dense v-model="item.since_now" @change="add" />
                </v-col>
            </v-row>
        </ListManagement>

        <ListManagement title="Skills" :names="['title']" :model="profile.skills" v-slot:default="{item, add}">
            <v-text-field label="Skill group name" dense v-model="item.title" @change="add" />
            <ListManagement :title="item.title" :names="['title', 'skill', 'years']" :model="item.subs" v-slot:default="{add, item}">
                <v-row slot="items">
                    <v-col>
                        <v-text-field label="Title" dense v-model="item.title" @change="add" />
                    </v-col>
                    <v-col>
                        <v-select dense v-model="item.skill" @change="add" :items="['Beginner', 'Advanced']">
                        </v-select>
                    </v-col>
                    <v-col>
                        <v-text-field label="Years" dense type="number" v-model="item.years" @change="add" />
                    </v-col>
                </v-row>
            </ListManagement>
        </ListManagement>

        <ListManagement title="Open source projects" :names="['title', 'descript', 'url']" :model="profile.opensource_projects" v-slot:default="{add, item}">
            <v-row slot="items">

                <v-col>
                    <v-text-field dense label="Title" v-model="item.title" @change="add" />
                </v-col>
                <v-col>
                    <v-text-field dense label="Descript" v-model="item.descript" @change="add" />
                </v-col>
                <v-col>
                    <v-text-field dense label="Url" v-model="item.url" @change="add" />
                </v-col>
            </v-row>
        </ListManagement>
    </v-expansion-panels>

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
    props: {
        model: Object
    },
    data() {
        return {
            input: Object,
            contact_types: [
                "Phone", "Email", "Telegram", "WhatsApp", "Twitter"
            ],
            profile: {
                personal_info: {},
                contact_info: [{}],
                educations: [{}],
                opensource_projects: [{}],
                jobs: [{}],
                skills: [{
                    subs: [{}]
                }],
            },
            rules: {
                date(d) {
                    if (d == "")
                        return "Invalid date";

                    if (!/\d{4}-\d{1,2}-\d{1,2}/.test(d))
                        return "Invalid date";

                    return true;
                }
            }
        }
    },
    methods: {
        save() {
            var filename = this.profile.personal_info.name + '-' + this.profile.personal_info.last_name + ".json";
            var content = JSON.stringify(this.profile);
            var file = new Blob([content], {
                type: "text"
            });
            if (window.navigator.msSaveOrOpenBlob) // IE10+
                window.navigator.msSaveOrOpenBlob(file, filename);
            else { // Others
                var a = document.createElement("a"),
                    url = URL.createObjectURL(file);
                a.href = url;
                a.download = filename;
                document.body.appendChild(a);
                a.click();
                setTimeout(function () {
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
        this.input.addEventListener("change", function () {
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

        // this.profile = JSON.parse(localStorage.profile || this.profile);
    }
}
</script>
