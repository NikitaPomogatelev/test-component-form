<template>
  <label class="form__label">
    <span class="form__label-name">Язык</span>
    <div
      class="select"
      :class="activeClass"
      @click="this.isOptionVisible = !this.isOptionVisible"
    >
      <input
        type="text"
        class="input select__input"
        :value="selected"
        placeholder="Язык"
      />

      <div class="select__options" v-if="isOptionVisible">
        <ul class="select__options-list">
          <li
            class="select__options-item"
            v-for="option in options"
            :key="option.value"
            @click="selectOption(option)"
          >
            {{ option.name }}
          </li>
        </ul>
      </div>
    </div>
  </label>
</template>

<script>
export default {
  name: 'app-custom-select',
  emits: ['selected'],
  props: {
    options: {
      type: Array,
      default() {
        return [];
      }
    },
    selected: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      isOptionVisible: false,
      isVisible: false
    };
  },
  methods: {
    selectOption(option) {
      this.$emit('selected', option);
      this.isOptionVisible = false;
    },
    hideSelect(event) {
      this.isOptionVisible = false;
      console.log(event);
    }
  },
  computed: {
    activeClass() {
      return this.isOptionVisible ? 'active' : '';
    }
  }
};
</script>

