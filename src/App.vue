<template>
<v-app :class="{preview: preview}">
    <v-app-bar app color="primary" dark>
        <div class="d-flex align-center">
            <!-- <v-img alt="Vuetify Logo" class="shrink mr-2" contain src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png" transition="scale-transition" width="40" />

            <v-img alt="Vuetify Name" class="shrink mt-1 hidden-sm-and-down" contain min-width="100" src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png" width="100" /> -->
            <h1>
            Resume maker
            </h1>
        </div>

        <v-spacer></v-spacer>

        <v-btn text>
            <v-icon dark>
                fa-download
            </v-icon>
            <span class="ml-2">Save</span>
        </v-btn>

        <v-btn text>
            <v-icon dark>
                fa-upload
            </v-icon>
            <span class="ml-2">Open</span>
        </v-btn>

        <v-btn text @click="print">
            <v-icon dark>
                fa-file-pdf
            </v-icon>
            <span class="ml-2">Export</span>
        </v-btn>

        <v-btn text @click="preview=!preview">
            <v-icon dark v-show="preview">
                fa-eye
            </v-icon>
            <v-icon dark v-show="!preview">
              fa-eye-slash
            </v-icon>
            <span class="ml-2">Preview</span>
        </v-btn>
    </v-app-bar>

    <v-main>
        <Main model="profile" v-show="!preview"/>
        <Preview model="profile" v-show="preview" />
    </v-main>
</v-app>
</template>

<script>
import Main from './components/Main.vue';
import Preview from './components/Preview.vue';

export default {
    name: 'App',

    components: {
        Main, Preview
    },

    data: () => ({
        preview: false,
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
    }),

    methods: {
      print() {
        window.print()
      }
    }
};
</script>

<style>
@import url(./style/common.scss);
</style>
<style>
.number-cell {
    vertical-align: middle;
    text-align: center;
    border-right: 1px solid red;
}

.v-card {
    margin-top: 12px;
    margin-bottom: 12px;
    padding: 12px;
}

.v-card h1 {
    border-bottom: 1px solid red;
}

.item-row:hover {
    background-color: yellow !important;
}
</style>
