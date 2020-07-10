<template>
    <v-container>
        <v-data-table
                :headers="headers"
                :items="desserts"
                sort-by="calories"
                class="elevation-1"
                :hide-default-footer="true"
                :item-class="item => getDesertColor(item)"
        >
            <template v-slot:top>
                <v-toolbar flat color="white">
                    <v-toolbar-title>Table</v-toolbar-title>
                    <v-divider
                            class="mx-4"
                            inset
                            vertical
                    ></v-divider>
                    <v-dialog v-model="dialog" max-width="500px">
                        <v-card>
                            <v-card-title>
                                <span class="headline">{{ formTitle }}</span>
                            </v-card-title>
                            <v-card-actions>
                                <v-spacer></v-spacer>
                                <v-btn color="green darken-1"
                                       text @click="save">
                                    Ок
                                </v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-dialog>
                </v-toolbar>
            </template>
            <template v-slot:item.actions="{ item }">
             <span v-if=item.status>
                <v-btn
                       color="green darken-3"
                       dark
                       class="mb-2"
                       v-bind="attrs"
                       v-on="on"
                       @click="openMessage(item)"
                >More
                </v-btn>
             </span>
            </template>
        </v-data-table>
    </v-container>
</template>

<script>
    export default {
        data: () => ({
            data: {
                disabled: true,
            },
            dialog: false,
            headers: [
                {
                    text: 'Dessert (100g serving)',
                    align: 'start',
                    sortable: false,
                    value: 'name',
                },
                {text: 'Calories', value: 'calories'},
                {text: 'Fat (g)', value: 'fat'},
                {text: 'Carbs (g)', value: 'carbs'},
                {text: 'Protein (g)', value: 'protein'},
                {text: 'Actions', value: 'actions', sortable: false},
            ],
            desserts: [],
            editedIndex: -1,
            editedItem: {
                name: '',
                calories: 0,
                fat: 0,
                carbs: 0,
                protein: 0,
            },
            defaultItem: {
                name: '',
                calories: 0,
                fat: 0,
                carbs: 0,
                protein: 0,
            },
        }),

        computed: {
            formTitle() {
                return this.editedIndex === -1 ? 'Сообщение об ошибке' : 'Сообщение об ошибке'
            },
        },

        watch: {
            dialog(val) {
                val || this.close()
            },
        },

        created() {
            this.initialize()
        },

        methods: {
            getDesertColor(dessert) {
                if (dessert.status === true) return "green"
                else return "red"
            },
            initialize() {
                this.desserts = [
                    {
                        name: 'Frozen Yogurt',
                        calories: 159,
                        fat: 6.0,
                        carbs: 24,
                        protein: 4.0,
                        status: true
                    },
                    {
                        name: 'Ice cream sandwich',
                        calories: 237,
                        fat: 9.0,
                        carbs: 37,
                        protein: 4.3,
                        status: false
                    },
                    {
                        name: 'Ice cream sandwich',
                        calories: 237,
                        fat: 9.0,
                        carbs: 37,
                        protein: 4.3,
                        status: false
                    },
                    {
                        name: 'Ice cream sandwich',
                        calories: 159,
                        fat: 9.0,
                        carbs: 37,
                        protein: 4.3,
                        status: true
                    },
                    {
                        name: 'Ice cream sandwich',
                        calories: 159,
                        fat: 9.0,
                        carbs: 37,
                        protein: 4.3,
                        status: true
                    },
                ]
            },

            openMessage(item) {
                this.editedIndex = this.desserts.indexOf(item)
                this.editedItem = Object.assign({}, item)
                this.dialog = true
            },

            close() {
                this.dialog = false
                this.$nextTick(() => {
                    this.editedItem = Object.assign({}, this.defaultItem)
                    this.editedIndex = -1
                })
            },

            save() {
                if (this.editedIndex > -1) {
                    Object.assign(this.desserts[this.editedIndex], this.editedItem)
                } else {
                    this.desserts.push(this.editedItem)
                }
                this.close()
            },
        },
    }
</script>