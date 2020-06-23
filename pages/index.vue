<template>
    <div>
    <v-app-bar
        fixed
        :color="(!scroll)? 'transparent' : layout.header.bgColor"
        :flat="!scroll"
    >
        <v-toolbar-items>
            <v-btn
                text
                class="px-0"
                aria-label="to-top"
                @click="$vuetify.goTo(0)"
            >
                <v-img
                    v-if="layout.header.logo"
                    :src="`${layout.header.logo.url}?fit=clip&h=56`"
                    alt=""
                ></v-img>
              
                <v-toolbar-title
                    v-if="layout.header.title"
                    :class="`${layout.header.txtColor}--text`"
                >
                    {{ layout.header.title }}
                </v-toolbar-title>
            </v-btn>
        </v-toolbar-items>
      
        <v-spacer></v-spacer>
        
        <v-menu
            v-if="$vuetify.breakpoint.xs"
            right
            nudge-right
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              :color="layout.header.txtColor"
              aria-label="menu"
              v-bind="attrs"
              v-on="on"
              icon
            >
                <v-icon>mdi-menu</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item
              v-for="(menu,i) in menus"
                :key="i"
                @click="move(menu.id)"
            >
              <v-list-item-title>{{ menu.title }}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
        
        <v-toolbar-items v-else>
            <v-btn
                v-for="(menu,i) in menus"
                :key="i"
                @click="move(menu.id)"
                color="transparent"
                :small="$vuetify.breakpoint.sm"
                :class="`${layout.header.txtColor}--text`"
                depressed
            >
                <span>{{ menu.title }}</span>
            </v-btn>
        </v-toolbar-items>
        
    </v-app-bar>
    <v-main v-scroll="onScroll">
        <template v-for="(section, i) in sections">
            <div v-if="section.menu" :id="`menu${i}`"></div>
            <v-img
                :src="(section.section.bgImg)? section.section.bgImg.url : undefined"
                :height="(section.section.type=='t')? '100vh' : undefined"
            >
                <v-sheet
                    :color="(section.section.bgColor)? section.section.bgColor : 'transparent'"
                    height="100%"
                    tile
                >
                    <v-container class="py-10" style="height:100%;min-height:50vh">
                        <v-row style="height:100%;" class="justify-center align-center">
                            <v-col
                                v-if="section.section.contents"
                                v-html="section.section.contents"
                                cols="12"
                                sm="10"
                            ></v-col>
                            <template v-if="section.section.type=='3c'">
                                <template v-if="section.section.item">
                                    <v-col
                                        cols="12"
                                        sm="8"
                                        md="4"
                                        v-for="(item,i) in section.section.item"
                                        :key="i"
                                        class="align-self-stretch"
                                    >
                                        <v-card
                                            v-if="item.contents"
                                            color="transparent"
                                            height="100%"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    v-html="item.contents"
                                                    :color="(item.bgColor)? item.bgColor : undefined"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-10"
                                                >
                                                    
                                                </v-sheet>
                                            </v-img>
                                        </v-card>
                                    </v-col>
                                </template>
                            </template>
                            <template v-else-if="section.section.type=='2t'">
                                <template v-if="section.section.item">
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        v-for="(item,i) in section.section.item"
                                        :key="i"
                                        class="px-3 px-sm-0 py-0 align-self-stretch"
                                    >
                                        <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                        <v-sheet
                                            :color="(item.bgColor)? item.bgColor : undefined"
                                            height="100%"
                                            width="100%"
                                            class="d-flex"
                                            tile
                                        >
                                            <v-card
                                                v-if="item.contents"
                                                v-html="item.contents"
                                                color="transparent"
                                                class="ma-auto"
                                                flat
                                                tile
                                            >
                                            </v-card>
                                        </v-sheet>
                                        </v-img>
                                    </v-col>
                                </template>
                            </template>
                            <template v-else-if="section.section.type=='3t'">
                                <template v-if="section.section.item">
                                    <v-col
                                        cols="12"
                                        md="4"
                                        v-for="(item,i) in section.section.item"
                                        :key="i"
                                        class="px-3 px-sm-0 py-0 align-self-stretch"
                                    >
                                        <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                        <v-sheet
                                            :color="(item.bgColor)? item.bgColor : undefined"
                                            height="100%"
                                            width="100%"
                                            class="d-flex"
                                            tile
                                        >
                                            <v-card
                                                v-if="item.contents"
                                                v-html="item.contents"
                                                color="transparent"
                                                class="ma-auto"
                                                flat
                                                tile
                                            >
                                            </v-card>
                                        </v-sheet>
                                        </v-img>
                                    </v-col>
                                </template>
                            </template>
                            <template v-else-if="section.section.type=='i'">
                                <v-col cols="12" sm="8" md="6">
                                    <v-carousel hide-delimiters cycle interval="3000" height="auto">
                                        <v-carousel-item
                                            v-for="(slide,i) in section.section.carousel"
                                            :key="i"
                                            :src="(slide.img)? slide.img.url : undefined"
                                        ></v-carousel-item>
                                    </v-carousel>
                                </v-col>
                            </template>
                            <template v-else-if="section.section.type=='m'">
                                <v-col v-if="section.section.movie" cols="12" sm="8" md="6">
                                    <div style="width: 100%;padding-bottom: 56.25%;height: 0px;position: relative;">
                                        <iframe
                                            style="position: absolute;top: 50%;left: 50%;transform: translate(-50%, -50%);-webkit-transform: translate(-50%, -50%);-ms-transform: translate(-50%, -50%);"
                                            width="100%"
                                            height="100%"
                                            title="Movie"
                                            :src="`https://www.youtube.com/embed/${section.section.movie}`"
                                            frameborder="0"
                                            allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                                            allowfullscreen
                                        ></iframe>
                                    </div>
                                </v-col>
                            </template>
                            <template v-else>
                            </template>
                            <v-col
                                v-if="section.section.btnLabel&&section.section.btnLink"
                                cols="12"
                                sm="10"
                                class="d-flex justify-center"
                            >
                                <v-btn
                                    :to="section.section.btnLink"
                                >
                                    {{ section.section.btnLabel }}
                                </v-btn>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-sheet>
            </v-img>
        </template>
    </v-main>
    <v-footer padless :color="layout.footer.bgColor">
        <v-sheet tile width="100%" color="transparent">
            <v-row class="mx-0 justify-center">
                <v-col cols="auto" :class="`${layout.footer.txtColor}--text caption`">
                    {{ layout.footer.copyright }}
                </v-col>
            </v-row>
        </v-sheet>
    </v-footer>
    </div>
</template>

<script>
export default {
    async asyncData ({ payload, app }) {
        if(payload) {
            return {
                sections: payload.sections,
                menus: payload.menus,
                layout: payload.layout
            }
        }
        else if(process.env.NODE_ENV !== 'production') {
            var layout = await app.$axios.$get(`https://testlp.microcms.io/api/v1/layout?depth=3`, {
                headers: { 'X-API-KEY': '000a5261-24c3-4f70-b87b-0d68d2ff256a' }
            })
            layout = layout.contents[0]
            var sections = layout.layout
            layout = {
                header: {
                    title: layout.title,
                    logo: layout.logo,
                    bgColor: layout.bgColorH,
                    txtColor: layout.txtColorH
                },
                footer: {
                    copyright: layout.copyright,
                    bgColor: layout.bgColorF,
                    txtColor: layout.txtColorF
                }
            }
            var menus = sections.map((x, i) => {
                if (x.menu) {
                    return {
                        id: i,
                        title: x.contents[0].title
                    }
                }
            })
            menus = menus.filter(x => x)
            return {
                    sections: sections,
                    menus: menus,
                    layout: layout
            }
        }
    },
    data() {
        return {
            scroll: false
        }
    },
    methods: {
        onScroll (e) {
            var top = window.pageYOffset ||e.target.scrollTop || 0
            if(top >= window.innerHeight) {
                this.scroll = true
            }
            else {
                this.scroll = false
            }
        },
        move (id) {
            return this.$vuetify.goTo(`#menu${id}`, this.option)
        }
    },
    computed: {
        option () {
            if (this.$vuetify.breakpoint.mdAndUp) {
                return {offset: 64}
            }
            else {
                return {offset: 56}
            }
        }
    }
}
</script>