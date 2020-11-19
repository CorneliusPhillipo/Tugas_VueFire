<template>
    <v-row class="fill-height overflow-auto" id="container">
        <v-col
            v-for="(item) in drinks"
            :key="item"
            :cols="(12/itemsPerRow)"
            class="py-2"
        > 
            <v-card
                class="mx-auto"
                max-width="344"
            >
                <v-img
                    :src="randomPhoto(item.name)"
                    height="200px"
                    alt=""
                ></v-img>

                <v-card-title>
                    {{item.icehot}} {{item.name}}
                </v-card-title>

                <v-card-actions>
                    <v-card-text>
                        Details
                    </v-card-text>
                    <v-spacer></v-spacer>

                    <v-btn
                        icon
                        @click="show = !show"
                        :key="item"
                    >
                        <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
                    </v-btn>
                </v-card-actions>

                <v-expand-transition>
                <div v-show="show">
                    <v-divider></v-divider>
                    <v-card-text>
                        <list>
Type:   &nbsp;&nbsp;&nbsp;&nbsp;{{item.icehot}}
Sugar:  &nbsp;&nbsp;&nbsp;&nbsp;{{item.sugar}}g
Water:  &nbsp;&nbsp;&nbsp;&nbsp;{{item.water}}ml
Price:  &nbsp;&nbsp;&nbsp;&nbsp;{{item.price}}ml
                        </list>
                    </v-card-text>
                </div>
                </v-expand-transition>
            </v-card>
        </v-col>
    </v-row>
</template>


<script>

    //tambahkan ini untuk import database reference
    import { bevRef } from '../firebase'

    export default {
        data: () => ({
            show: false,
            photos: [
                'https://images.unsplash.com/photo-1489619447385-3f6a391029c6?ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80',
                'https://images.unsplash.com/photo-1551024709-8f23befc6f87?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2025&q=80',
                'https://images.unsplash.com/photo-1497534446932-c925b458314e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2014&q=80',
                'https://images.unsplash.com/photo-1486428263684-28ec9e4f2584?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2134&q=80',
                'https://images.unsplash.com/photo-1558383409-ab7ef8db3e01?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1970&q=80',
                'https://images.unsplash.com/photo-1582106245687-cbb466a9f07f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80',
                'https://images.unsplash.com/photo-1598679253587-829c6cc6c6fc?ixlib=rb-1.2.1&auto=format&fit=crop&w=2134&q=80',
           
            ],
            selected: null,
            drinks: [],
            editedIndex: -1,
            colorCache: {},
            imgCache: {},
        }),

       created () {
            this.selected = this.randomPhoto(this.photos);
            window.addEventListener('resize', () => {
                this.calcRowsPerPage()
            })
       },

       firebase: {
           drinks: bevRef,
       },

       computed: {
            numberOfPages () {
                return Math.ceil(this.drinks.length / this.ipp);
            },
            rowsPerPage () {
                return this.rpp;
            },
            itemsPerRow () {
                switch (this.$vuetify.breakpoint.name) {
                    case 'xs': return 1;
                    case 'sm': return 2;
                    case 'md': return 3;
                    case 'lg': return 4;
                    case 'xl': return 5;
                }
                return 0;
            },
            ipp () {
                return Math.ceil(this.rowsPerPage * this.itemsPerRow);
            },
        },

        methods: {
            calcRowsPerPage () {
                let container = document.getElementById('container')
                let minItemHeight = 150
                if (container) {
                    let containerHeight = parseInt(container.clientHeight, 0)
                    this.rpp = Math.floor(containerHeight/minItemHeight)
                }
                else {
                    this.rpp = 4
                }
            },
            randomPhoto(id) {
                return this.imgCache[id] || (this.imgCache[id] = this.photos[Math.floor(Math.random() * this.photos.length)]);
            },
        },
    }
</script>