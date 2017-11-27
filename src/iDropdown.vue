<template>
  <div class="dropdown" role="button" @mousedown.prevent="toggleDropdown">
    <input value="1" type="hidden" v-model="model">
    <span ref="selected">
      {{ model }}
    </span>
    <i ref="dropdownIcon" role="presentation" class="open-indicator">
      
    </i>

    <!-- <input ref="search" v-model="search" @keydown.delete="maybeDeleteValue" @keyup.esc="onEscape" @keydown.up.prevent="typeAheadUp"
        @keydown.down.prevent="typeAheadDown" @keydown.enter.prevent="typeAheadSelect" @blur="onSearchBlur" @focus="onSearchFocus"
        type="search" class="form-control" :disabled="disabled" :placeholder="searchPlaceholder" :tabindex="tabindex" :readonly="!searchable"
        :style="{ width: isValueEmpty ? '100%' : 'auto' }" :id="inputId" aria-label="Search for option"> -->

    <!-- <slot name="spinner">
        <div class="spinner" v-show="mutableLoading">Loading...</div>
      </slot> -->


    <transition>
      <ul v-if="dropdownOpen" class="dropdown-menu" :style="{ 'max-height': maxHeight }">
        <li v-for="(option, index) in filteredOptions" v-bind:key="index" :class="{ active: isOptionSelected(option), highlight: index === typeAheadPointer }"
          @mouseover="typeAheadPointer = index">
          <a @mousedown.prevent="select(option)">
            <slot name="option" v-bind="option">
              {{ getOptionLabel(option) }}
            </slot>
          </a>
        </li>
        <li v-if="!filteredOptions.length" class="no-options">
          <slot name="no-options">Sorry, no matching options.</slot>
        </li>
      </ul>
    </transition>
    <!-- <div class="menu ui transition" :class="{hidden: !showDropdown}">
      <div class="item active" data-value="1">
        <a @mousedown.prevent="select('Male')">
          Male
        </a>
      </div>
      <div class="item" data-value="0">
        <a @mousedown.prevent="select('Female')">
          Female
        </a>
      </div>
    </div> -->
  </div>
</template>

<style lang="scss" src="./iDropdown.scss" scoped></style>

<script>
export default {
  name: 'md-button',
  components: {},
  data: () => ({
    dropdownOpen: false,
  }),
  props: {
    /**
       * Contains the currently selected value. Very similar to a
       * `value` attribute on an <input>. You can listen for changes
       * using 'change' event using v-on
       * @type {Object||String||null}
       */
    value: {
      default: null,
    },
    /**
       * An optional callback function that is called each time the selected
       * value(s) change. When integrating with Vuex, use this callback to trigger
       * an action, rather than using :value.sync to retreive the selected value.
       * @type {Function}
       * @param {Object || String} val
       */
    onChange: {
      type: Function,
      default: function(val) {
        this.$emit('input', val);
      },
    },
    /**
       * Sets the max-height property on the dropdown list.
       * @deprecated
       * @type {String}
       */
    maxHeight: {
      type: String,
      default: '400px',
    },
  },
  computed: {
    model: {
      get() {
        return this.value;
      },
      set(v) {
        this.$emit('input', v);
      },
    },
  },
  methods: {
    /**
       * Toggle the visibility of the dropdown menu.
       * @param  {Event} e
       * @return {void}
       */
    toggleDropdown(e) {
      if (
        e.target === this.$refs.dropdownIcon ||
        // e.target === this.$refs.search ||
        e.target === this.$refs.selected ||
        e.target === this.$el
      ) {
        if (this.open) {
          // this.$refs.search.blur(); // dropdown will close on blur
        } else {
          if (!this.disabled) {
            this.open = true;
            // this.$refs.search.focus();
          }
        }
      }
    },
    toggle() {
      this.dropdownOpen = !this.dropdownOpen;
    },

    /**
       * Select a given option.
       * @param  {Object|String} option
       * @return {void}
       */
    select(option) {
      this.model = option;
    },
  },
};
</script>