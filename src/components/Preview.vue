<template>
    <v-container class="main">
        Preview:
        {{model.personal_info.name}}
        {{model.personal_info.last_name}}

        <hr />
        {{model}}

        <h1>Open source projects</h1>
        <div v-for="(p, i) in model.opensource_projects" :key="i">
            {{p.url}}
        </div>
    </v-container>
</template>

<script>
export default {
    props: {
        model: Object
    },    
}
</script>