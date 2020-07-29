<template>
    <v-container>

        <v-text-field
                style="margin-left: 70%"
                v-model="filterDessertParameter"
                clearable
                hide-details
                prepend-inner-icon="search"
                label="Найти..."
        @input="search">
        </v-text-field>
        <v-data-table
                :headers="headers"
                :items="filteredDessertsList"
                sort-by="calories"
                class="elevation-1"
                :hide-default-footer="true"
                :item-class="item => getDesertColor(item)"
        >
            <template v-slot:top>
                    <v-container grid-list-md>
                        <v-layout row wrap>
                            <v-flex xs12 lg6>
                                <v-menu
                                        v-model="menu2"
                                        :close-on-content-click="true"
                                        lazy
                                        transition="scale-transition"
                                        offset-y
                                        full-width
                                        min-width="290px"
                                >

                                    <template v-slot:activator="{ on }">
                                        <v-text-field
                                                v-model="data.dateStart"
                                                label="Start"
                                                prepend-icon="event"
                                                readonly
                                                v-on="on"
                                        ></v-text-field>
                                    </template>
                                    <v-date-picker v-model="data.dateStart"
                                                   @input="menu2 = false"
                                                   first-day-of-week="1"
                                                   color="green"
                                    >

                                    </v-date-picker>
                                </v-menu>
                            </v-flex>
                                <v-flex xs12 lg6>
                                    <v-menu
                                            v-model="menu2"
                                            :close-on-content-click="true"
                                            lazy
                                            transition="scale-transition"
                                            offset-y
                                            full-width
                                            min-width="290px"
                                    >
                                        <template v-slot:activator="{ on }">
                                            <v-text-field
                                                    v-model="data.dateEnd"
                                                    label="End"
                                                    prepend-icon="event"
                                                    readonly
                                                    v-on="on"
                                            ></v-text-field>
                                        </template>
                                        <v-date-picker v-model="data.dateEnd"
                                                       @input="menu2 = false"
                                                       first-day-of-week="1"
                                                       color="green"
                                        >
                                        </v-date-picker>
                                    </v-menu>
                                </v-flex>

                        </v-layout>
                    </v-container>
                <v-toolbar flat color="white">
                    <v-toolbar-title>Table</v-toolbar-title>

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
                       style="top:5px"
                >More
                </v-btn>
            </template>
        </v-data-table>
    </v-container>
</template>

<script>
    export default {
        data: () => ({
            filter: {},
            sortBy: 'name',
            keys: [
                'Uuid',
                'Статус',
                'Дата',
                'Ошибки',
            ],
            data: {
                disabled: true,
                dateStart: new Date().toISOString().substr(0, 10),
                dateEnd: new Date().toISOString().substr(0, 10),
                menu2: false,
            },
            dialog: false,
            headers: [
                {
                    text: 'OID организации',
                    align: 'start',
                    sortable: false,
                    value: 'oid',
                },
                {text: 'Uuid', value: 'uuid'},
                {text: 'Статус', value: 'stat'},
                {text: 'Дата', value: 'dateCreate', sortable: false},
                {text: 'Ошибки', value: 'info', sortable: false},
            ],
            desserts: [],
            editedItem: {
                oid: '',
                uuid: 0,
                stat: 0,
                createDate: null
            },
            defaultItem: {
                oid: '',
                uuid: 0,
                stat: 0,
            },
            filterDessertParameter: "",
            filteredDessertsList: [],
        }),
        computed: {
            formTitle() {
                return 'Code {{ number }}'
            },
            filteredKeys () {
                return this.keys.filter(key => key !== `Name`)
            },
        },

        watch: {
            dialog(val) {
                val || this.close()
            },
        },

        created() {
            this.initialize();

            let dates = this.desserts.map(dessert => dessert.dateCreate);
            this.data.dateStart = new Date(
                Math.min.apply(null, dates)
            ).toISOString().substr(0, 10);

            this.data.dateEnd = new Date (
                Math.max.apply(null, dates)
            ).toISOString().substr(0, 10);


        },

        methods: {
            search() {
                if (!this.filterDessertParameter) {
                    this.filteredDessertsList = this.desserts
                } else {
                    this.filteredDessertsList = this.desserts.filter(dessert => {
                        return dessert.oid(this.filteredDessertsList)
                        // dessert.oid.contains(this.filterParameter) ||
                        // dessert.uuid.contains(this.filterParameter) ||
                        // dessert.stat.contains(this.filterParameter) ||
                        // dessert.status.contains(this.filterParameter) ||
                        // dessert.dateCreate.toDateString().contains(this.filterParameter)
                    })
                }
            },
            getDesertColor(dessert) {
                if (dessert.status === true) return "green"
                else return "red"
            },
            initialize() {
                this.desserts = [
                    {
                        oid: '1.2.643.6.17.1',
                        uuid: '0d809103-85a5-467a-a629-76f569ca0c07',
                        stat: 'Ок',
                        status: true,
                        dateCreate: new Date('2020, 5, 9')
                    },
                    {
                        oid: '1.2.643.6.7',
                        uuid: '716ce0c1-8cb0-4811-8dd9-eb64e0a186a6',
                        stat: 'Ошибка',
                        status: false,
                        dateCreate: new Date('2020, 6, 13')
                    },
                    {
                        oid: '1.2.643.6.15',
                        uuid: 'd0467040-4ae5-4152-8289-0806f9784d76',
                        stat: 'Ошибка',
                        status: false,
                        dateCreate: new Date('2020, 5, 17')
                    },
                    {
                        oid: '1.2.643.3.241',
                        uuid: '8e41fb4e-34ee-41f3-9a31-35385b5bd4e0',
                        stat: 'Ок',
                        status: true,
                        dateCreate: new Date('2019, 3, 2')
                    },
                    {
                        oid: '1.2.643.0.2',
                        uuid: 'bfb0ae42-b178-4128-9576-23dbd1025646',
                        stat: 'Ок',
                        status: true,
                        dateCreate: new Date('2021, 2, 7')
                    },
                ]
                this.filteredDessertsList = this.desserts;
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
