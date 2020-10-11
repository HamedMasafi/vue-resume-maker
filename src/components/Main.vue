<template>
<v-container class="main">
    <v-expansion-panels>
        <v-expansion-panel>
            <v-expansion-panel-header>
              Personal Info
            </v-expansion-panel-header>
            <v-expansion-panel-content>
                <v-text-field label="Name" dense v-model="model.personal_info.name" />
                <v-text-field label="Last" dense v-model="model.personal_info.last_name" />
                <v-text-field label="EMail" dense v-model="model.personal_info.email" />
                <v-text-field label="GitHub address" dense v-model="model.personal_info.github" />
            </v-expansion-panel-content>
        </v-expansion-panel>

        <ListManagement title="Contact info" :names="['type', 'value']" :model="model.contact_info" v-slot:default="{add, item}">
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

        <ListManagement title="Educations" :names="['name', 'from', 'to']" :model="model.educations" v-slot:default="{add, item}">
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

        <ListManagement title="Jobs" :names="['name', 'position', 'from', 'to', 'since_now']" :model="model.jobs" v-slot:default="{add, item}">
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

        <ListManagement title="Skills" :names="['title']" :model="model.skills" v-slot:default="{item, add}">
            <v-text-field label="Skill group name" dense v-model="item.title" @change="add" />
            <v-expansion-panels>
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
            </v-expansion-panels>
        </ListManagement>

        <ListManagement title="Open source projects" :names="['title', 'descript', 'url']" :model="model.opensource_projects" v-slot:default="{add, item}">
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
    }
}
</script>
