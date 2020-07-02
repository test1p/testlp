<template>
    <v-sheet
        :color="(section.bgColor)? section.bgColor : color.section.bg"
        tile
        width="100%"
        class="d-flex justify-center"
    >
    <v-img
        :src="(bgImg)? bgImg : undefined"
        :height="(section.max)? '100vh' : undefined"
        :style="(bgImg)? 'background: white;' : undefined"
        :max-width="(this.$vuetify.breakpoint.lgAndUp)? '1264px' : '100vw'"
    >
        <v-sheet
            :color="(bgImg && section.bgColor)? section.bgColor : 'transparent'"
            tile
            height="100%"
        >
            <v-container :id="section.id" :class="(!$route.params.id)? 'py-10' : 'pt-2 pb-10 px-sm-0'" style="height:100%;min-height:50%;">
                <v-row style="height:100%;" class="justify-center align-center">
                    <v-col
                        v-if="section.contents"
                        v-html="section.contents"
                        cols="12"
                        sm="10"
                    ></v-col>
                    <template v-if="section.item">
                        <template v-if="section.slide">
                            <v-col cols="12" sm="8" md="6">
                                <v-carousel
                                    cycle
                                    height="auto"
                                    show-arrows-on-hover
                                    hide-delimiter-background
                                >
                                    <v-carousel-item
                                        v-for="(item,i) in section.item"
                                        :key="i"
                                    >
                                        <v-card
                                            color="transparent"
                                            class="ma-2"
                                            :flat="!item.card"
                                            :tile="!item.card"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    v-if="item.contents"
                                                    v-html="item.contents"
                                                    :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-10"
                                                ></v-sheet>
                                            </v-img>
                                        </v-card>
                                    </v-carousel-item>
                                </v-carousel>
                            </v-col>
                        </template>
                        <template v-else>
                            <template v-for="item in section.item">
                                <v-col
                                    v-if="!item.expand"
                                    cols="12"
                                    :sm="(section.col)? 6 : 6"
                                    :md="(section.col)? 4 : 6"
                                    :class="(item.card)? 'align-self-stretch px-4 px-sm-3' : 'align-self-stretch px-4 px-sm-0 py-0'"
                                >
                                    <template v-if="item.card">
                                        <v-card
                                            color="transparent"
                                            height="100%"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-5 pa-sm-7"
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                    ></div>
                                                    <v-row class="py-2 justify-center">
                                                        <v-btn
                                                            v-if="item.btnLabel && item.btnLink"
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            outlined
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-sheet>
                                            </v-img>
                                        </v-card>
                                    </template>
                                    <template v-else>
                                        <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                            <v-sheet
                                                :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                height="100%"
                                                width="100%"
                                                class="d-flex"
                                                tile
                                            >
                                                <v-card
                                                    color="transparent"
                                                    class="ma-auto pa-5 pa-sm-7"
                                                    flat
                                                    tile
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                    ></div>
                                                    <v-row class="py-2 justify-center">
                                                        <v-btn
                                                            v-if="item.btnLabel && item.btnLink"
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            :style="`color:${color.btn.txt};`"
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-card>
                                            </v-sheet>
                                        </v-img>
                                    </template>
                                </v-col>
                                <v-col
                                    v-else
                                    cols="12"
                                    sm="10"
                                    :class="(item.card)? 'align-self-stretch px-4 px-sm-3' : 'align-self-stretch px-4 px-sm-0 py-0'"
                                >
                                    <template v-if="item.card">
                                        <v-card
                                            color="transparent"
                                            height="100%"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-10"
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                    ></div>
                                                    <v-row class="py-2 justify-center">
                                                        <v-btn
                                                            v-if="item.btnLabel && item.btnLink"
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            outlined
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-sheet>
                                            </v-img>
                                        </v-card>
                                    </template>
                                    <template v-else>
                                        <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                            <v-sheet
                                                :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                height="100%"
                                                width="100%"
                                                class="d-flex"
                                                tile
                                            >
                                                <v-card
                                                    color="transparent"
                                                    class="ma-auto pa-3"
                                                    width="100%"
                                                    flat
                                                    tile
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                    ></div>
                                                    <v-row class="py-2 justify-center">
                                                        <v-btn
                                                            v-if="item.btnLabel && item.btnLink"
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            :style="`color:${color.btn.txt};`"
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-card>
                                            </v-sheet>
                                        </v-img>
                                    </template>
                                </v-col>
                            </template>
                        </template>
                    </template>
                    <v-col v-if="section.movie" cols="12" sm="8" md="6">
                        <div style="width: 100%;padding-bottom: 56.25%;height: 0px;position: relative;">
                            <iframe
                                style="position: absolute;top: 50%;left: 50%;transform: translate(-50%, -50%);-webkit-transform: translate(-50%, -50%);-ms-transform: translate(-50%, -50%);"
                                width="100%"
                                height="100%"
                                title="Movie"
                                :src="`https://www.youtube.com/embed/${section.movie}`"
                                frameborder="0"
                                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                                allowfullscreen
                            ></iframe>
                        </div>
                    </v-col>
                    <v-col
                        v-if="section.btnLabel&&section.btnLink"
                        cols="12"
                        sm="10"
                        class="d-flex justify-center"
                    >
                        <v-btn
                            @click="move(section.btnLink)"
                            :color="color.btn.bg"
                            :style="`color:${color.btn.txt};`"
                        >
                            {{ section.btnLabel }}
                        </v-btn>
                    </v-col>
                    <v-col
                        v-if="section.contact"
                        cols="12"
                        sm="10"
                    >
                        <v-form
                            name="contact"
                            method="POST"
                            data-netlify="true"
                            netlify-honeypot="bot-field"
                            action="/"
                        >
                            <input type="hidden" name="form-name" value="contact">
                                <v-radio-group
                                    v-model="radio"
                                    row
                                >
                                    <v-radio
                                        v-for="radio in radios"
                                        :key="radio.label"
                                        :label="radio.label"
                                        :value="radio.value"
                                        name="overview"
                                    ></v-radio>
                                </v-radio-group>
                            <v-combobox
                                v-model="service"
                                :items="items"
                                :background-color="color.contact.input"
                                label="対象プランの選択・入力"
                                name="service[]"
                                multiple
                                outlined
                            ></v-combobox>
                            <v-text-field
                                v-model="corporateName"
                                :background-color="color.contact.input"
                                label="団体名・屋号"
                                name="corporate-name"
                                outlined
                            ></v-text-field>
                            <v-text-field
                                v-model="contactName"
                                :background-color="color.contact.input"
                                label="ご担当者様名"
                                name="contact-name"
                                outlined
                            ></v-text-field>
                            <v-text-field
                                v-model="email"
                                :background-color="color.contact.input"
                                label="メールアドレス"
                                name="email"
                                outlined
                            ></v-text-field>
                            <v-textarea
                                v-model="content"
                                :background-color="color.contact.input"
                                label="内容"
                                name="content"
                                outlined
                            ></v-textarea>
                            <v-text-field
                                v-model="botField"
                                label="人間は入力しないでください"
                                name="bot-field"
                                v-show="false"
                            />
                            <v-btn
                                type="submit"
                                :color="color.btn.bg"
                                :style="`color:${color.btn.txt};`"
                            >
                                送信
                            </v-btn>
                        </v-form>
                    </v-col>
                </v-row>
            </v-container>
        </v-sheet>
    </v-img>
    </v-sheet>
</template>

<script>

export default {
    data() {
        return {
            color: {
                section: {bg: 'rgba(242,242,242,1)'},
                item: {bg: 'white'},
                btn: {bg: 'rgba(102,153,204,1)', txt: 'white' },
                contact: {input: 'white'}
            },
            service: [],
            items: [
                'ベーシック',
                'スタンダード',
                'プレミアム',
                'カスタマイズ',
                'その他',
            ],
            radio: 'request',
            radios: [
                {label: 'お申込', value: 'request'},
                {label: 'ご質問', value: 'question'},
                {label: 'お見積', value: 'estimation'},
            ],
            corporateName: '',
            contactName: '',
            email: '',
            content: '' ,
            botField: ''
        }
    },
    methods: {
        move (link) {
            if (link.includes('http')) {
                return window.open(link, '_blank')
            }
            else if (link.includes('#')) {
                return this.$vuetify.goTo(link, this.option)
            }
            else {
                return this.$router.push(link)
            }
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
        },
        bgImg () {
            if (this.section.bgImg2) {
                var bgImg = this.section.bgImg2.url
                if (this.$vuetify.breakpoint.xs) {
                    if (this.section.bgImg) {
                        bgImg = this.section.bgImg.url
                    }
                    return `${bgImg}?fit=clip&w=600`
                }
                else if (this.$vuetify.breakpoint.sm) {
                    return `${bgImg}?fit=clip&w=960`
                }
                else {
                    return `${bgImg}?fit=clip&w=1264`
                }
            }
        }
    },
    props: ['section']
}
</script>