<template>
    <v-app id="inspire">
        <v-navigation-drawer
            v-model="drawerRight"
            app
            clipped
            right
        >
            <v-list dense>
                <v-list-item @click.stop="right = !right">
                    <v-list-item-action>
                        <v-icon>mdi-phone</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title></v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <v-app-bar
            app
            clipped-right
            dark
            color="indigo"
        >
            <v-app-bar-nav-icon @click.stop="drawerRight = !drawerRight" />
            <v-toolbar-title>داشبورد</v-toolbar-title>
            <v-spacer />
            <v-text-field
                solo-inverted
                flat
                hide-details
                label="جستجو در کارها"
                dark
                prepend-inner-icon="mdi-search"
            />
            <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
            <!--<template v-slot:extension>-->
                <!--<v-tabs-->
                    <!--background-color="transparent"-->
                    <!--right-->
                    <!--icons-and-text-->
                <!--&gt;-->
                    <!--<v-tab><v-icon>mdi-label</v-icon></v-tab>-->
                    <!--<v-tab><v-icon>mdi-label</v-icon></v-tab>-->
                    <!--<v-tab><v-icon>mdi-phone</v-icon></v-tab>-->
                <!--</v-tabs>-->
            <!--</template>-->
        </v-app-bar>

        <v-navigation-drawer
            v-model="drawer"
            app
        >
            <v-list>
                <v-list-item @click.stop="left = !left">
                    <v-avatar class="ml-3" size="32">
                        <img
                            :src="'/storage/avatar/' + user.avatar"
                            :alt="user.name"
                        >
                    </v-avatar>
                    <!--<v-list-item-content>-->
                        <!--<v-list-item-title>{{user.name}}</v-list-item-title>-->
                    <!--</v-list-item-content>-->
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <v-navigation-drawer
            v-model="left"
            fixed
            temporary
        >
        </v-navigation-drawer>

        <v-content>
            <v-container
                class="fill-height"
                fluid
            >
                <v-row
                    justify="center"
                    align="center"
                >
                    <v-col class="shrink">


                    </v-col>
                </v-row>
            </v-container>
        </v-content>

        <v-navigation-drawer
            v-model="right"
            fixed
            right
            temporary
            style="width: 500px"
        >
            <contact user></contact>

        </v-navigation-drawer>

        <v-footer
            app
            color="indigo"
            class="white--text"
        >
            <v-avatar class="ml-3">
                <img
                    :src="'/storage/avatar/' + user.avatar"
                    :alt="user.name"
                >
            </v-avatar>
            <span>{{user.name}}</span>
            <v-spacer />
            <span>&copy; 2020</span>
        </v-footer>
    </v-app>
</template>

<script>
    export default {
        props:['user'],
        data: () => ({
            drawer: null,
            drawerRight: null,
            right: false,
            left: false,
        }),
        mounted: function () {

        // this.changeRTL()

        },
        methods: {
            // changeRTL () {
            //     this.$vuetify.rtl = true
            // },
        },
    }
</script>
