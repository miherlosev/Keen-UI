<template>
    <div id="app" class="keen-docs">
        <sidebar class="is-desktop"></sidebar>

        <transition name="transition-fade">
            <div
                class="keen-docs-mobile-sidebar__backdrop"
                @click="showSidebar = false"
                v-show="showSidebar"
            ></div>
        </transition>

        <transition name="transition-slide">
            <sidebar class="is-mobile" v-show="showSidebar"></sidebar>
        </transition>

        <section class="keen-docs-content">
            <div class="keen-docs-content__toolbar">
                <div class="keen-docs-content__toolbar-content">
                    <ui-icon-button
                        class="keen-docs-content__toolbar-menu-button"
                        color="white"
                        type="clear"

                        @click="showSidebar = true"
                    >
                        <ui-icon>
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M3 6h18v2.016H3V6zm0 6.984v-1.97h18v1.97H3zM3 18v-2.016h18V18H3z"/></svg>
                        </ui-icon>
                    </ui-icon-button>

                    <h1 class="keen-docs-content__toolbar-title">{{ $route.meta.title }}</h1>

                    <a
                        class="keen-docs-content__toolbar-action"
                        rel="noopener"
                        target="_blank"

                        :href="'https://github.com/JosephusPaye/Keen-UI/blob/master/' + $route.meta.sourceUrl"

                        v-if="$route.meta.sourceUrl"
                    >View Source</a>
                </div>
            </div>

            <div class="keen-docs-content__page-content" ref="pageContent">
                <router-view></router-view>
            </div>
        </section>
    </div>
</template>

<script>
import Sidebar from './Sidebar.vue';
import UiIcon from 'src/UiIcon.vue';
import UiIconButton from 'src/UiIconButton.vue';

export default {
    data() {
        return {
            showSidebar: false,
            description: 'Keen UI - A lightweight collection of essential UI components written with Vue and inspired by Material Design.'
        };
    },

    watch: {
        '$route'() {
            this.updatePageTitle();

            this.$nextTick(() => {
                window.Prism.highlightAll();
                this.$refs.pageContent.scrollTop = 0;
                this.showSidebar = false;
            });
        }
    },

    mounted() {
        this.updatePageTitle();
    },

    methods: {
        updatePageTitle() {
            document.title = this.$route.meta.title + ' | ' + this.description;
        }
    },

    components: {
        Sidebar,
        UiIcon,
        UiIconButton
    }
};
</script>

<style lang="scss">
@import './styles/index.scss';
</style>
