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
            <template v-slot:item.info="{ item }">
                <v-btn v-if=!item.status
                       color="orange darken-1"
                       dark
                       class="mb-2"
                       v-bind="attrs"
                       v-on="on"
                       @click="openMessage(item)"
                >More
                </v-btn>
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
                    text: 'Message',
                    align: 'start',
                    sortable: false,
                    value: 'name',
                },
                {text: 'Uuid', value: 'uuid'},
                {text: 'Status', value: 'stat'},
                {text: 'Errors', value: 'info', sortable: false},
            ],
            desserts: [],
            editedItem: {
                name: '',
                uuid: 0,
                stat: 0,
            },
            defaultItem: {
                name: '',
                uuid: 0,
                stat: 0,
            },
        }),

        computed: {
            formTitle() {
                return 'Code { number }'
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
                        name: 'Message 1',
                        uuid: '0d809103-85a5-467a-a629-76f569ca0c07',
                        stat: 'Success',
                        status: true
                    },
                    {
                        name: 'Message 2',
                        uuid: '716ce0c1-8cb0-4811-8dd9-eb64e0a186a6',
                        stat: 'Error',
                        status: false
                    },
                    {
                        name: 'Message 3',
                        uuid: 'd0467040-4ae5-4152-8289-0806f9784d76',
                        stat: 'Error',
                        status: false
                    },
                    {
                        name: 'Message 4',
                        uuid: '8e41fb4e-34ee-41f3-9a31-35385b5bd4e0',
                        stat: 'Success',
                        status: true
                    },
                    {
                        name: 'Message 5',
                        uuid: 'bfb0ae42-b178-4128-9576-23dbd1025646',
                        stat: 'Success',
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
