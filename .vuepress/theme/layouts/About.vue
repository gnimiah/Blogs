<template>
  <Common class="about-container" :sidebar="false">
    <div class="about-banner-wrapper">
      <div>
        <ModuleTransition>
          <h1 v-if="recoShowModule">
            <span>不积跬步，无以至千里</span>
          </h1>
        </ModuleTransition>
      </div>
      <component v-if="bubbles" :is="bubbles"></component>
    </div>
    <div class="about-content-wrapper">
      <ModuleTransition delay="0.08">
        <section v-show="recoShowModule">
          <Content/>
        </section>
      </ModuleTransition>
    </div>
  </Common>
</template>

<script>
import {defineComponent, toRefs, reactive, computed, onMounted} from 'vue-demi'
import Common from '@theme/components/Common'
import {ModuleTransition} from '@vuepress-reco/core/lib/components'
import moduleTransitonMixin from '@theme/mixins/moduleTransiton'
import {useInstance} from '@theme/helpers/composable'

export default defineComponent({
  mixins: [moduleTransitonMixin],
  components: {Common, ModuleTransition},
  setup(props, ctx) {
    const instance = useInstance()

    const state = reactive({
      bubbles: null
    })

    onMounted(() => {
      import('vue-canvas-effect/src/components/bubbles').then(module => {
        state.bubbles = module.default
      })
    })

    return {...toRefs(state)}
  },
})
</script>
<style src="../styles/theme.styl" lang="stylus"></style>
<style src="prismjs/themes/prism-tomorrow.css"></style>
<style lang="stylus" scoped>
.about-container
  .about-banner-wrapper
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    width: 100%;
    height: calc(50vh);
    margin: $navbarHeight auto 0;
    color: #fff;
    font-size: 1.6rem;
    overflow: hidden;
    background: url("/about-me-bg.jpg") center center / cover no-repeat;

    h1 {
      display: block;
      margin: 0 auto 1.8rem;
      font-size: 2.5rem;
      color: #fff;
    }

  .about-content-wrapper
    display: flex;
    max-width: $contentWidth;
    align-items: flex-start;
    margin: 0 auto;
    padding: 20px;
    opacity: 1;
</style>
