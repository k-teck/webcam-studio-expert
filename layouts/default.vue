<template>
  <div class="page-body">
    <Header />
    <Nuxt class="content" />
    <div v-if="$store.state.modals.current" class="modal-wrapper">
      <div class="modal-wrapper--inner">
        <component :is="component" />
        <button class="close-btn" @click="$closeModal" />
      </div>
    </div>
    <Footer v-if="!$store.getters.isMobile" />
  </div>
</template>

<script>
import Header from '@/components/Header'

export default {
  name: 'StudiosLayout',
  components: {
    Header,
  },
  data() {
    return {
      component: null,
    }
  },
  computed: {
    modal() {
      return this.$store.state.modals.current
    },
  },
  watch: {
    modal(newV, oldV) {
      this.$store.commit('modals/setPrev', oldV)
      this.component = () => import(`@/components/modals/${this.modal}.vue`)
    },
    $route() {
      this.$scrollToTop()
    },
  },
}
</script>

<style lang="scss">
#__nuxt {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

#__layout {
  width: 100%;
  height: 100%;
  overflow: auto;
  scroll-behavior: smooth;

  &::-webkit-scrollbar {
    display: none;
  }
}

.page-body {
  // display: grid;
  // grid-template-rows: auto 1fr auto;
  // min-height: 100vh;
}

.content {
  position: relative;
  top: 60px;
  min-height: 100vh;
}

.modal-wrapper {
  position: absolute;
  width: 100%;
  top: 0;
  bottom: 0;
  overflow: auto;
  z-index: 2;
}
.modal-wrapper--inner {
  min-height: 100vh;
  position: relative;
  display: flex;
  justify-content: center;
  padding: 80px 0;
  overflow: hidden;
  &::before {
    content: '';
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    z-index: -1;
    background: linear-gradient(90deg, #c45792 0%, #7f53c4 100%);
    opacity: 0.6;
  }
  & > *:not(button) {
    width: 350px;
    height: max-content;
    background-color: #fbfbfd;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 24px 28px;
    overflow: auto;
  }
  .close-btn {
    width: 40px;
    height: 40px;
    position: absolute;
    top: 80px;
    right: 50%;
    transform: translateX(175px);
    background-color: rgba(255, 255, 255, 0);
    &:active {
      transform: translateY(2px) translateX(175px);
    }
  }
}

@media screen and (min-width: 420px) {
  .content {
    top: 150px;
  }
}
</style>
