<template>
    <div>
    <v-list>
        <v-subheader>فعالیت ها
            <v-spacer></v-spacer>
            <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                    <v-switch v-model="switchX" v-on="on" @change="intervalSwap"></v-switch>
                </template>
                <span>بروزرسانی خودکار</span>
            </v-tooltip>
        </v-subheader>


        <!--<v-expansion-panels flat>-->
            <!--<v-expansion-panel>-->
                <!--<v-expansion-panel-header>فیلتر</v-expansion-panel-header>-->
                <!--<v-expansion-panel-content>-->
                    <!--<v-switch v-model="switch1" :label="`فقط نظرات`"></v-switch>-->
                    <!--<v-switch v-model="switch2" :label="`فقط نظرات من`"></v-switch>-->
                    <!--<v-switch v-model="switch2" :label="`نظرات را نمایش نده`"></v-switch>-->

                <!--</v-expansion-panel-content>-->
            <!--</v-expansion-panel>-->
        <!--</v-expansion-panels>-->




        <v-skeleton-loader
            :loading="loading"
            :transition="transition"
            height="94"
            type="list-item-avatar-two-line"
            v-if="items.length<=0"
        >
        </v-skeleton-loader>
        <v-container
            style="max-height: 70vh"
            class="overflow-y-auto"
            dark
        >
        <template v-for="(item, index) in items" class="overflow-hidden">
              <v-list-item
                :key="index"
                @click="" v-on="on"
            >
                <v-list-item-avatar>
                    <v-img :src="'/storage/avatar/' + item.user.avatar"></v-img>
                </v-list-item-avatar>

                <v-list-item-content>
                    <v-list-item-subtitle v-text="item.note" class="text-right text-sm"></v-list-item-subtitle>
                    <v-list-item-subtitle v-html="item.diff" class="text-right text-sm"></v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>
        </template>
        </v-container>



    </v-list>
    </div>
</template>

<script>
    export default {
        name: "ActivityReportList",
        props:['user','qty'],
        data: () => ({
            switch1: true,
            switch2: false,
            switchX:true,
            loading: false,
            transition: 'scale-transition',
            items:[],
            timeToInterval:''
        }),
        mounted() {
              this.intervalSwap()
        },
        methods:{
            intervalSwap(){
                if (this.switchX){
                    this.timeToInterval = setInterval(() => {
                        this.read()
                    }, 3000);
                } else {
                    window.clearInterval(this.timeToInterval)
                }

            },
            read(){
                    let url = '/api/activity/read?uId=' + this.user.id + '&qty=' + this.qty;
                    axios.get(url)
                        .then(response =>this.items = response.data)
                        .catch(function (error) {
                            console.log(error)
                        });
            }
        }
    }
</script>

<style scoped>

</style>
