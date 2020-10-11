<template>
<v-container class="print-template">
    <h1>
        Personal infoamations
    </h1>
    <section>
        Name: {{model.personal_info.name}} {{model.personal_info.last_name}}

        <h2>
            Contact informations:
        </h2>
        <div v-for="(c, i) in model.contact_info" :key="i">
            {{c.type}}: {{c.value}}
        </div>
    </section>

    <h1>Language</h1>
    <section class="language">
        <div v-for="(l, i) in model.language" :key="i">
            <span>
                {{l.language}}
            </span>
            <span>
                {{l.skill}}
            </span>
            <span>
                {{l.license}}
            </span>
        </div>
    </section>

    <h1>Skills</h1>
    <section class="skills">
        <div v-for="(s, i) in model.skills" :key="i">
            <h2>{{s.title}}</h2>
            <div v-for="(si, i) in s.subs" :key="i">
                <span>
                    {{si.title}}
                </span>
                <span>
                    {{si.skill}}
                </span>
                <span>
                    {{si.years}}
                </span>
            </div>
        </div>
    </section>

    <h1>Open source projects</h1>
    <section>
        <div v-for="(p, i) in model.opensource_projects" :key="i">
            <h2>{{p.title}}</h2>
            <p>
                {{p.descript}}
            </p>
            <a :href="p.url">{{p.url}}</a>
        </div>
    </section>

</v-container>
</template>

<script>
export default {
    props: {
        model: Object
    },
}
</script>
