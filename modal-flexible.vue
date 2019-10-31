<template>
  <transition name="fade">
    <div
      v-show="visible"
      ref="modal"
      class="speed-up modal-base"
    >
      <div
        class="backdrop"
        @click="$emit('hideModal')"
      />
      <transition
        name="fade-in-down"
        @after-enter="opened"
        @after-leave="closed"
      >
        <section
          v-if="visible"
          class="speed-up modal-container"
          :class="`modal-${size}`"
        >
          <div class="modal-header">
            <slot name="header">
              <slot name="header-text">
                <h3 class="modal-title" v-html="title" />
              </slot>

              <button
                v-if="topClose"
                class="close"
                @click="$emit('hideModal')"
              >
                <slot name="topClose">
                  <span class="fas fa-times" />
                </slot>
              </button>
            </slot>
          </div>

          <div class="modal-cnt">
            <slot name="content">
              just a single content
            </slot>
          </div>

          <div class="modal-footer row">
            <slot name="footer">
              <div class="col-xs-6 col-sm-5">
                <button
                  v-if="btnCancelText"
                  class="btn btn-danger w-100"
                  @click="$emit('hideModal')"
                >
                  {{ btnCancelText }}
                </button>
              </div>
              <div class="col-xs-6">
                <button
                  v-if="btnSubmitText"
                  class=" btn btn-success btn-loading"
                  :disabled="submitDisabled||isLoading"
                  @click="$emit('submitModal')"
                >
                  <i :class="{loading: isLoading}" />
                  {{ btnSubmitText }}
                </button>
              </div>
            </slot>
          </div>
        </section>
      </transition>
    </div>
  </transition>
</template>
<script>
  export default {
    props: {
      topClose: {
        type: Boolean,
        default: true
      },
      visible: {
        type: Boolean,
        default: false
      },
      title: {
        type: String,
        default: 'Type your Custom Title'
      },
      size: {
        type: String,
        default: 'medium'
      },
      btnCancelText: {
        type: String,
        default: ''
      },
      btnSubmitText: {
        type: String,
        default: ''
      },
      isLoading: {
        type: Boolean,
        default: false
      },
      submitDisabled: {
        type: Boolean,
        default: false
      }
    },

    methods: {
      opened() {
        document.addEventListener('keyup', this.escape);
        this.$emit('opened');
      },
      closed() {
        document.removeEventListener('keyup', this.escape);
        this.$emit('closed');
      },
      escape(evt) {
        if(evt.keyCode === 27) {
          this.$emit('hideModal');
        }
      }
    }
  };
</script>
<style lang="scss" scoped>
  @import '~@scss/common/modal-base.scss';
</style>
