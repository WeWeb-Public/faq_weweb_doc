<!-- This is a Vue.js single file component. -->
<!-- Check the Vue.js doc here :  -->
<!-- https://vuejs.org/v2/guide/ -->

<!-- This is your HTML -->
<template>
    <div class="faq">
        <!-- wwManager:start -->
        <wwSectionEditMenu v-bind:sectionCtrl="sectionCtrl"></wwSectionEditMenu>
        <!-- wwManager:end -->

        <div class="header">
            <wwObject class="image" ww-category="background" v-bind:ww-object="section.data.headerBg"></wwObject>
        </div>

        <div class="container">
            <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="section.data.headerContent" class="list" @ww-add="add(section.data.headerContent, $event)" @ww-remove="remove(section.data.headerContent, $event)">
                <wwObject tag="div" v-for="object in section.data.headerContent" :key="object.uniqueId" :ww-object="object"></wwObject>
            </wwLayoutColumn>
            
            <div class="card-container">
                <div class="card-elem" v-for="(card, index) in section.data.cards" :key="card.id" :class="{ cursor: card.link }" @click="goToPage(card.link)">
                    <!-- wwManager:start -->
                    <wwContextMenu class="ww-orange-button" tag="div" :options="cardOptions" @link="cardLink(index)" @remove="remove(section.data.cards, { index })" @addBefore="cardAdd(index)" @addAfter="cardAdd(index+1)" v-if="sectionCtrl.getEditMode() == 'CONTENT'">
                        <wwOrangeButton></wwOrangeButton>
                    </wwContextMenu>
                    <!-- wwManager:end -->
                    <wwLayoutColumn class="list" tag="div" ww-default="ww-text" :ww-list="card.content" @ww-add="add(card.content, $event)" @ww-remove="remove(card.content, $event)">
                        <wwObject tag="div" v-for="object in card.content" :key="object.uniqueId" :ww-object="object"></wwObject>
                    </wwLayoutColumn>
                </div>
            </div>

            <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="section.data.faqContent" class="list" @ww-add="add(section.data.faqContent, $event)" @ww-remove="remove(section.data.faqContent, $event)">
                <wwObject tag="div" v-for="object in section.data.faqContent" :key="object.uniqueId" :ww-object="object"></wwObject>
            </wwLayoutColumn>

            <div class="faq-container">
                <div class="faq-elem" v-for="(elem, index) in section.data.faq" :key="elem.id" :class="{ selected: faqSelected === elem.id }">
                    <!-- wwManager:start -->
                    <wwContextMenu class="ww-orange-button" tag="div" :options="faqOptions" @select="faqSelect(index)" @remove="remove(section.data.faq, { index })" @addBefore="faqAdd(index)" @addAfter="faqAdd(index+1)" v-if="sectionCtrl.getEditMode() == 'CONTENT'">
                        <wwOrangeButton></wwOrangeButton>
                    </wwContextMenu>
                    <!-- wwManager:end -->
                    <div class="faq-header" @click="faqSelect(index)">
                        <wwObject :ww-object="elem.title"></wwObject>
                        <div class="icon">
                            <wwObject class="ww-icon" :ww-object="elem.icon"></wwObject>
                        </div>
                    </div>
                    <div class="faq-content" :class="{ selected: faqSelected !== elem.id }">
                        <div class="faq-content-container">
                            <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="elem.content" class="elem" @ww-add="add(elem.content, $event)" @ww-remove="remove(elem.content, $event)">
                                <wwObject tag="div" v-for="object in elem.content" :key="object.uniqueId" :ww-object="object"></wwObject>
                            </wwLayoutColumn>
                        </div>
                    </div>
                </div>
            </div>

            <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="section.data.bottomContent" class="list" @ww-add="add(section.data.bottomContent, $event)" @ww-remove="remove(section.data.bottomContent, $event)">
                <wwObject tag="div" v-for="object in section.data.bottomContent" :key="object.uniqueId" :ww-object="object"></wwObject>
            </wwLayoutColumn>
        </div>
    </div>
</template>

<!-- This is your Javascript -->
<!-- ✨ Here comes the magic ✨ -->
<script>
export default {
    name: "__COMPONENT_NAME__",
    props: {
        // The section controller object is passed. You can access it anytime
        sectionCtrl: Object
    },
    data() {
        return {
            faqSelected: undefined,
            /* wwManager:start */
            cardOptions: {
                items: [
                    {
                        text: {
                            en: 'Before',
                            fr: 'Avant'
                        },
                        icon: 'wwi wwi-add',
                        action: 'addBefore'
                    },
                    {
                        text: {
                            en: 'After',
                            fr: 'Apres'
                        },
                        icon: 'wwi wwi-add',
                        action: 'addAfter'
                    },
                    {
                        text: {
                            en: 'Delete',
                            fr: 'Supprimer'
                        },
                        icon: 'wwi wwi-delete',
                        action: 'remove'
                    },
                    {
                        text: {
                            en: 'Link',
                            fr: 'Lien'
                        },
                        icon: 'wwi wwi-internal-link',
                        action: 'link'
                    }
                ]
            },
            faqOptions: {
                items: [
                    {
                        text: {
                            en: 'Select',
                            fr: 'Selectionner'
                        },
                        icon: 'wwi wwi-check',
                        action: 'select'
                    },
                    {
                        text: {
                            en: 'Before',
                            fr: 'Avant'
                        },
                        icon: 'wwi wwi-add',
                        action: 'addBefore'
                    },
                    {
                        text: {
                            en: 'After',
                            fr: 'Apres'
                        },
                        icon: 'wwi wwi-add',
                        action: 'addAfter'
                    },
                    {
                        text: {
                            en: 'Delete',
                            fr: 'Supprimer'
                        },
                        icon: 'wwi wwi-delete',
                        action: 'remove'
                    }
                ]
            }
            /* wwManager:end */
        }
    },
    computed: {
        // The section object contains all the info and data about the section
        // Use it as you like !
        section() {
            return this.sectionCtrl.get();
        }
    },
    created() {
        // Initialize the data once the section is created in the DOM
        this.init()
    },
    methods: {
        goToPage(pageId) {
            const path = wwLib.wwWebsiteData.getPageRoute(pageId, true) || '/';
            wwLib.goTo(path);
            this.$emit('next', null);
        },
        init() {
            // Initialize section data
            let needUpdate = false

            // We will only save the data in this.section.data
            // So you need to put in there all the data of you WeWeb objects
            this.section.data = this.section.data || {};

            if (!this.section.data.headerBg) {
                this.section.data.headerBg = wwLib.wwObject.getDefault({
                    type: 'ww-color'
                });
                needUpdate = true
            }

            if (!this.section.data.headerContent) {
                this.section.data.headerContent = []
                needUpdate = true
            }

            if (!this.section.data.bottomContent) {
                this.section.data.bottomContent = []
                needUpdate = true
            }

            if (!this.section.data.faqContent) {
                this.section.data.faqContent = []
                needUpdate = true
            }

            if (!this.section.data.faq) {
                this.section.data.faq = []
                this.faqAdd(0)
                needUpdate = true
            }

            if (!this.section.data.cards) {
                this.cardAdd(0)
                this.cardAdd(0)
                needUpdate = true
            }

            if (needUpdate) {
                this.sectionCtrl.update(this.section);
            }
        },
        faqSelect(index) {
            try {
                if (this.faqSelected === this.section.data.faq[index].id)
                    this.faqSelected = undefined
                else
                    this.faqSelected = this.section.data.faq[index].id

                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        // --------- EDITOR FUNCTIONS ---------
        // All the codes between /* wwManager:start */ and /* wwManager:end */ are only for editor purposes
        // So It won't in the published website!
        /* wwManager:start */
        faqAdd(index) {
            try {
                const data = {
                    id: wwLib.wwUtils.getUniqueId(),
                    title: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                    icon: wwLib.wwObject.getDefault({ type: 'ww-icon' }),
                    content: []
                }
                this.section.data.faq.splice(index, 0, data);
                this.faqSelect(index)
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        async cardLink(index) {
            let options = {
                firstPage: 'LINK_INTERNAL',
                data: { linkPage: this.section.data.cards[index].link }
            }
            try {
                const result = await wwLib.wwPopups.open(options)
                if (typeof(result.linkPage) !== 'undefined') {
                    this.section.data.cards[index].link = result.linkPage
                }
                this.sectionCtrl.update(this.section)
            } catch (err) {
                wwLib.wwLog.error(err)
            }
        },
        cardAdd(index) {
            try {
                const data = {
                    id: wwLib.wwUtils.getUniqueId(),
                    title: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                    icon: wwLib.wwObject.getDefault({ type: 'ww-icon' }),
                    content: []
                }
                this.section.data.cards.splice(index, 0, data);
                this.faqSelect(index)
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        add(list, options) {
            try {
                list.splice(options.index, 0, options.wwObject);
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        remove(list, options) {
            try {
                list.splice(options.index, 1);
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        }
        /* wwManager:end */
    }
};
</script>

<!-- This is your CSS -->
<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- Add lang="scss" or others to use computed CSS -->
<style lang="scss" scoped>
// --------- GLOBAL CSS ---------
.cursor {
    cursor: pointer;
}

// --------- FAQ CSS ---------
.faq {
    position: relative;
    .background {
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
    }
    .relative {
        position: relative;
    }
    .header {
        position: absolute;
        width: 100%;
        height: 180px;
        border-bottom-left-radius: 50%;
        border-bottom-right-radius: 50%;
        -webkit-clip-path: ellipse(70% 70% at 50% 30%);
                clip-path: ellipse(70% 70% at 50% 30%);
        @media (min-width: 768px) {
            height: 250px;
        }
        @media (min-width: 992px) {
            height: 300px;
        }
        @media (min-width: 1200px) {
            height: 400px;
        }
        .image {
            position: absolute;
            top: 0;
            left: 0;
            height: 100%;
            width: 100%;
        }
    }
    .card-container {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: center;
            -ms-flex-pack: center;
                justify-content: center;
        .card-elem {
            position: relative;
            display: -webkit-box;
            display: -ms-flexbox;
            display: flex;
            -webkit-box-align: center;
                -ms-flex-align: center;
                    align-items: center;
            -webkit-box-pack: center;
                -ms-flex-pack: center;
                    justify-content: center;
            pointer-events: all;
            -webkit-box-shadow: 0 1px 23px -5px rgba(0,0,0,0.2);
                    box-shadow: 0 1px 23px -5px rgba(0,0,0,0.2);
            background: white;
            max-width: 350px;
            width: -webkit-fit-content;
            width: -moz-fit-content;
            width: fit-content;
            width: 80%;
            border-radius: 5px;
            z-index: 1;
            margin: 10px;
            transition: transform 0.5s;
            @media (min-width: 768px) {
                margin: 50px;
            }
            .list {
                width: 100%;
            }
            &:hover {
                transform: translateY(-10px);
            }
        }
    }
    .faq-container {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-align: center;
            -ms-flex-align: center;
                align-items: center;
        -webkit-box-orient: vertical;
        -webkit-box-direction: normal;
            -ms-flex-direction: column;
                flex-direction: column;
        .faq-elem {
            position: relative;
            display: -webkit-box;
            display: -ms-flexbox;
            display: flex;
            -ms-flex-wrap: wrap;
                flex-wrap: wrap;
            pointer-events: all;
            background: #FAFAFA;
            width: 80%;
            max-width: 800px;
            border-radius: 5px;
            z-index: 1;
            margin: 15px;
            -webkit-transition: all .5s ease;
            transition: all .5s ease;
            .faq-header {
                display: -webkit-box;
                display: -ms-flexbox;
                display: flex;
                pointer-events: all;
                -webkit-box-pack: justify;
                    -ms-flex-pack: justify;
                        justify-content: space-between;
                width: 100%;
                -webkit-box-align: center;
                    -ms-flex-align: center;
                        align-items: center;
                cursor: pointer;
                .icon {
                    margin: 0 10px 0 25px;
                    .ww-icon {
                        -webkit-transition: all .5s;
                        transition: all .5s;
                    }
                }
            }
            .faq-content {
                display: -webkit-box;
                display: -ms-flexbox;
                display: flex;
                overflow: hidden;
                width: 100%;
                &:after {
                    content: '';
                    height: 0px;
                    -webkit-transition: all .5s;
                    transition: all .5s;
                    max-height: 0px;
                }
                .faq-content-container {
                    -webkit-transition: all .5s;
                    transition: all .5s;
                    display: -webkit-box;
                    display: -ms-flexbox;
                    display: flex;
                    -webkit-box-align: center;
                    -ms-flex-align: center;
                    align-items: center;
                    margin-bottom: 0;
                    max-height: 1000px;
                    min-height: 25px;
                    width: 100%;
                    .elem {
                        display: block;
                        margin: 0;
                        width: 100%;
                    }
                }
                &.selected > .faq-content-container {
                    margin-bottom: -1000px;
                    -webkit-transition: all 1.5s;
                    transition: all 1.5s;
                    visibility: hidden;
                    max-height: 0;
                    -webkit-transition: margin-bottom .5s cubic-bezier(1, 0, 1, 1),
                                visibility 0s elem-container, 
                                max-height 0s .5s;
                    transition: margin-bottom .5s cubic-bezier(1, 0, 1, 1),
                                visibility 0s .5s, 
                                max-height 0s .5s;
                }
                &.selected:after {
                    height: 0;
                    -webkit-transition: all .5s;
                    transition: all .5s;
                    max-height: 0px;
                }
            }
            &:hover {
                -webkit-box-shadow: 0 1px 23px -5px rgba(0,0,0,0.2);
                        box-shadow: 0 1px 23px -5px rgba(0,0,0,0.2);
            }
            &.selected {
                -webkit-box-shadow: 0 1px 23px -5px rgba(0,0,0,0.2);
                        box-shadow: 0 1px 23px -5px rgba(0,0,0,0.2);
                background: white;
                .faq-header {
                    .icon {
                        .ww-icon {
                            -webkit-transform: rotate(90deg);
                                    transform: rotate(90deg);
                        }
                    }
                }
                .faq-content {
                    // min-height: 500px;
                    // height: auto;
                }
            }

        }
    }
}

// --------- EDITOR CSS ---------
/* wwManager:start */
.faq {
    .ww-orange-button {
        position: absolute;
        top: 0;
        left: 0;
        -webkit-transform: translate(-50%, -50%);
                transform: translate(-50%, -50%);
        z-index: 2;
    }
}
/* wwManager:end */
</style>
