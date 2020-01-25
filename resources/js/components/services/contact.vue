<template>
    <v-app>
        <v-card>
            <v-card>
                <v-toolbar flat>
                    <v-text-field
                    class="m-3 mt-5"
                    v-model="search"
                    placeholder="جستجوی نام"
                    clearable
                    hint="برای مثال: محمد حریری"
                    dense
                    ></v-text-field>
                </v-toolbar>
                <v-container
                    style="max-height: 90vh; min-height: 400px;"
                    class="overflow-y-auto"
                    dark
                >
                    <v-list flat>
                    <v-list-item-group v-model="item" color="primary">
                    <v-list-item
                    v-for="(item, i) in items"
                    :key="i"
                    >
                    <v-list-item-content>
                    <v-list-item-title v-text="item.text"></v-list-item-title>
                    </v-list-item-content>
                    <v-spacer></v-spacer>
                    <v-chip>
                    {{item.tel}}
                    </v-chip>
                    </v-list-item>
                    </v-list-item-group>
                    </v-list>
                    <v-dialog v-model="addDialog" persistent max-width="600px">
                        <template v-slot:activator="{ on }">
                            <v-btn
                                absolute
                                dark
                                fab
                                bottom
                                right
                                color="indigo"
                                v-on="on"
                            >
                                <v-icon>mdi-plus</v-icon>
                            </v-btn>
                        </template>
                        <v-card>
                            <v-card-title>
                                <h4>اطلاعات تماس</h4>
                            </v-card-title>
                            <v-card-text>
                                <v-container>
                                    <v-row>
                                        <v-col cols="12" sm="6">
                                            <v-text-field
                                                v-model="name"
                                                :rules="nameRules"
                                                label="نام*"
                                                required
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" sm="6">
                                            <v-text-field
                                                v-model="organization"
                                                label="سمت / سازمان" hint="برای مثال: واحد فروش"></v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="4">
                                            <v-text-field
                                                v-model="mobile"
                                                :rules="telRules"
                                                label="موبایل"
                                                type="number"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="4">
                                            <v-text-field
                                                v-model="tel"
                                                :rules="telRules"
                                                label="تلفن"
                                                type="number"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="4">
                                            <v-text-field
                                                v-model="intercom"
                                                :rules="telRules"
                                                label="داخلی"
                                                type="number"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="6" v-if="more">
                                            <v-text-field
                                                v-model="email"
                                                :rules="emailRules"
                                                label="پست الکترونیک"
                                                type="email"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="6" v-if="more">
                                            <v-select
                                                :items="['سایر' ,'داخلی']"
                                                label="گروه"
                                            ></v-select>
                                        </v-col>
                                        <v-col cols="12" md="6" v-if="more">
                                            <v-text-field
                                                type="text"
                                                v-model="date"
                                                id="my-custom-editable-input"
                                                label="تاریخ تولد"
                                                @click="show=true"
                                            ></v-text-field>
                                            <date-picker
                                                v-model="date"
                                                format="jYYYY/jMM/jDD"
                                                element="my-custom-editable-input"
                                                :editable="true"
                                                :show="show"
                                                @close="show=false"
                                                view="year"
                                                type="date"
                                            />
                                        </v-col>
                                        <v-col cols="12" md="6" v-if="more">
                                            <v-text-field
                                                v-model="web"
                                                label="وبسایت"
                                            ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" v-if="more">
                                        <v-textarea
                                            clearable
                                            clear-icon="cancel"
                                            label="آدرس"
                                            model="address"
                                            rows="1"
                                        ></v-textarea>
                                        </v-col>
                                        <v-col cols="12" v-if="more">
                                        <v-textarea
                                            clearable
                                            clear-icon="cancel"
                                            label="توضیحات"
                                            model="note"
                                            rows="2"
                                        ></v-textarea>
                                        </v-col>

                                    </v-row>
                                </v-container>
                                <small>*ضروری ها ستاره دارد</small>
                            </v-card-text>
                            <v-card-actions>
                                <v-btn color="secondary darken-1" text @click="more=!more" v-if="!more"><v-icon>mdi-chevron-down</v-icon></v-btn>
                                <v-btn color="secondary darken-1" text @click="more=!more" v-if="more"><v-icon>mdi-chevron-up</v-icon></v-btn>
                                <v-spacer></v-spacer>
                                <v-btn color="success" text @click="addDialog = false"><v-icon>mdi-content-save</v-icon> ذخیره</v-btn>
                                <v-dialog v-model="confirmDialog" persistent max-width="290">
                                    <template v-slot:activator="{ on }">
                                        <v-btn color="dark" text v-on="on"><v-icon>mdi-close</v-icon></v-btn>
                                    </template>
                                    <v-card>
                                        <v-card-title class="headline">از بستن این پنجره اطمینان دارید؟</v-card-title class="headline">
                                        <v-card-actions>
                                            <v-spacer></v-spacer>
                                            <v-btn color="warning darken-1" text @click="confirmDialog = false">خیر</v-btn>
                                            <v-btn color="green darken-1" text @click="addDialog = false">بله</v-btn>
                                        </v-card-actions>
                                    </v-card>
                                </v-dialog>
                            </v-card-actions>
                        </v-card>
                    </v-dialog>
                </v-container>
            </v-card>
        </v-card>
    </v-app>
</template>

<script>
    export default {
        props:['user'],
        name: "contact",
        data: () => ({
            more:false,
            items: [
                { text: 'هادی رفیعی', tel: '406' },
            ],
            tabs: null,
            search:'',
            addDialog: false,
            confirmDialog: false,
            name: '',
            nameRules: [
                v => !!v || 'نام ضروری است',
            ],
            email: '',
            emailRules: [
                v => !!v || 'ایمیل ضروری است',
                v => /.+@.+/.test(v) || 'ایمیل صحیح وارد کنید',
            ],
            organization:'',
            mobile:'',
            tel:'',
            intercom:'',
            date: '',
            web: '',
            address: '',
            note: '',
            show: false,
        }),
        watch: {
            menu (val) {
                val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
            },
        },
        methods: {
            save (date) {
                this.$refs.menu.save(date)
            },
        },
        mounted() {
        }
    }
</script>

<style scoped>

</style>
