<template>
    <div>
        <nut-row class="mobile-btn-panel-title">
            <nut-col :span="24">{{$t('buttons.mainTitle')}}</nut-col>
        </nut-row>
        <nut-row style="margin-bottom: 2rem;">
            <nut-col :span="24"
                     style="text-align: center">
                <nut-switch :active.sync="isListMode">
                </nut-switch>
                <span style="margin-left: 1rem">
                    {{$t('buttons.playListMode')}}
                </span>
            </nut-col>
        </nut-row>

        <LiveInfo title-size="1.5rem" class="live-info-panel"></LiveInfo>

        <el-card v-for="(group, index) in btnGroups" :key="index" class="group">
            <div slot="header">
                <span style="font-size: 1.5rem;">{{group.group_name.lang[lang]}}</span>
            </div>
            <nut-row type="flex" flex-wrap="wrap">
                <nut-button class="sound-btn"
                            type="light"
                            v-for="(btn, i) in group.buttons"
                           :key="i"
                           @click="play(btn)">
                    {{btn.name.lang[lang]}}
                </nut-button>
            </nut-row>
        </el-card>
    </div>
</template>

<script>
    import {mapState} from 'vuex'
    import {Player} from '../../utils/player'
    import {ADD_ORDER} from '../../store/mutation-types'
    import {AUDIO_PREFIX} from '../../utils/constants'
    import LiveInfo from '../LiveInfo'

    export default {
        name: "Buttons",
        components: {
            LiveInfo
        },
        data() {
            return {
                isListMode: false,
                sourcePrefix: AUDIO_PREFIX
            }
        },
        computed: {
            lang() {
                return this.$i18n.locale
            },
            ...mapState([
                'btnGroups',
                'egg',
                'volume'
            ])
        },
        methods: {
            play(item) {
                if (this.isListMode) {
                    this.$store.commit(ADD_ORDER, item)
                }
                const player = new Player(this.sourcePrefix, this.volume)
                player.play(item.path)
            }
        }
    }
</script>

<style lang="scss" scoped>
@import '../../styles/index';

.mobile-btn-panel-title {
    text-align: center;
    font-size: 2rem
}

.sound-btn {
    margin-bottom: 0.5rem;
    margin-right: 0.5rem;
    padding: 0 10px;
    background-color: $ahoy-primary-color;
    color: $ahoy-text-color;
}

.live-info-panel {
    font-size: 14px !important;
    margin: 0.5rem;
}

.group {
    margin: 0.5rem;
}
</style>