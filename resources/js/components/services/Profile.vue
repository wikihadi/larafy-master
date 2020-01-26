<template>
    <v-app>
        <v-card
            class="mx-auto"
            max-width="100%"
            outlined
        >
            <v-avatar class="" size="250">
                <img
                    :src="'/storage/avatar/' + user.avatar"
                    :alt="user.name"
                >
            </v-avatar>
            <v-card-title class="text-right">{{user.name}}</v-card-title>

            <!--<v-card-subtitle class="pb-0" class="text-right">{{user.id}}</v-card-subtitle>-->

            <v-card-text class="text--primary text-right">
                <div>{{user.email}}</div>
            </v-card-text>
        </v-card>
    </v-app>
</template>

<script>
    export default {
        name: "Profile",
        props:['user'],
        mounted: function () {

        },
        methods: {

        },
    }
</script>

<style scoped>

</style>
