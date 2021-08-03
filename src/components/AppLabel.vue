<template>
  <label class="form__label" :class="errorClass">
    <span class="form__label-name">{{ name }}</span>
    <input
      class="input form-input"
      :value="value"
      :type="type"
      :placeholder="placeholder"
      @input="onInput"
    />
    <transition v-if="activated" name="fade" mode="in-out" appear>
      <span v-show="!valid" class="form__label-error"
        >Введено не корректное значение</span
      >
    </transition>
  </label>
</template>

<script>
export default {
  data() {
    return {
      activated: this.value !== ''
    }
  },
  props: {
    name: {
      type: String,
      required: true
    },
    type: {
      type: String,
      default: 'text'
    },
    value: {
      type: String
    },
    valid: {
      type: Boolean
    },

    placeholder: {
      type: String,
      required: true
    }
  },
  methods: {
    onInput(e) {
      this.activated = true
      this.$emit('updated', e.target.value)
    }
  },
  computed: {
    errorClass() {
      return this.valid ? '' : 'active'
    }
  }
}
</script>

<style lang="scss">
.icon-enter-active {
  animation: iconIn 0.3s;
}

.icon-leave-active {
  animation: iconOut 0.3s;
}

.icon-appear-active {
  animation: iconFade 0.3s;
  // animation: iconOut 0.3s;
}

@keyframes iconIn {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

@keyframes iconOut {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes iconFade {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
