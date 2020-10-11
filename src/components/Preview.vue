<template>
    <div>
        {{model.persinal_info.name}}
        {{model.persinal_info.last_name}}
    </div>
</template>

<script>
export default {
    props: {
        model: Object
    },    
}
</script>