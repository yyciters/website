<template>
    <transition name="move">
        <div v-if="showAlbumDetail" class="album-detail" >
        <!-- <div class="album-detail" > -->
            <img class="cover-img" :src="curAlbumImgSrc" alt="">
            <span @click="closeAlbumDetail" class="icon-close-wrap">
                <CloseIcon class="icon icon-close" />
            </span>
                <div class="album-songs">
            <VuePerfectScrollbar class="scroll-area" :settings="settings">
                    <song-item
                        v-for="(song, index) in curAlbum.songs"
                        :key="song._id"
                        :song="song"
                        :index="index"
                    ></song-item>
            </VuePerfectScrollbar>
                </div>
        </div>
    </transition>
</template>

<script>
import { mapState, mapGetters } from 'vuex';
import VuePerfectScrollbar from 'vue-perfect-scrollbar'
import SongItem from './song-item.vue';
import CloseIcon from "../../static/img/close.svg";

export default {
    name: 'album-detail',
    data () {
        return {
            settings: {
                maxScrollbarLength: 60
            }
        }
    },
    components: {
        SongItem,
        CloseIcon,
        VuePerfectScrollbar,
    },
    computed: {
        ...mapState([
            'showAlbumDetail',
            'curAlbum',
            'curAlbumImgSrc'
        ]),
        ...mapGetters([
            'songsList'
        ])
    },
    created () {
        console.log('this.curAlbum================>', this.curAlbum)
    },
    methods: {
        closeAlbumDetail () {
            this.$store.commit('setWillPlayList', this.songsList)
            this.$store.commit('setShowAlbumDetail', false)
            this.$store.commit("setHasMask", false)
        }
    }
}
</script>

<style lang="less" scoped>
    @import  (reference) "../less/mixin.less";
    .album-detail {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        position: absolute;
        z-index: 1;
        top: 0;
        left: @musicBoxNavWidth;
        width: calc(100% - @musicBoxNavWidth);
        height: 100%;
        background-color: #ffffff;

         
        
        .cover-img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .icon-close-wrap {
            position: absolute;
            cursor: pointer;
            z-index: 100;
            right: 20px;
            top: 20px;
            width: 24px;
            height: 24px;
            display: inline-block;
            border-radius: 2px;
            background: rgba(0, 0, 0, .1);
        }

        .icon-close {
            width: 24px;
            height: 24px;
        }

        .album-songs {

            flex-flow: nowrap; // 防止高度被撑开
            z-index: 20;
            // margin-top: 300px;
            padding: 20px;
            background-color: rgba(0, 0, 0, .3);
            width: 500px;
            min-height: 0;
            border-radius: @borderRadius;

            .scroll-area {
                position: relative;
                width: 100%;
                height: 400px;
            }
        }
    }
</style>

