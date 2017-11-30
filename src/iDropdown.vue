<template>
  <div class="dropdown">
    <div ref="toggle" role="button" @mousedown.prevent="openDropdown" :class="{inline}">
      <input value="1" type="hidden" v-model="model">
      <span ref="selected">
        {{ getLabel(mutableValue) }}
      </span>

      <slot name="icon">
        <i role="presentation" class="i-arrow_drop_down"></i>
      </slot>
    </div>
    <!-- <input ref="search" v-model="search" @keydown.delete="maybeDeleteValue" @keyup.esc="onEscape" @keydown.up.prevent="typeAheadUp"
        @keydown.down.prevent="typeAheadDown" @keydown.enter.prevent="typeAheadSelect" @blur="onSearchBlur" @focus="onSearchFocus"
        type="search" class="form-control" :disabled="disabled" :placeholder="searchPlaceholder" :tabindex="tabindex" :readonly="!searchable"
        :style="{ width: isValueEmpty ? '100%' : 'auto' }" :id="inputId" aria-label="Search for option"> -->

    <!-- <slot name="spinner">
        <div class="spinner" v-show="mutableLoading">Loading...</div>
      </slot> -->


    <transition name="fade">
      <section v-show="dropdownOpen">
        <!-- <input ref="search" v-model="search" @keydown.delete="maybeDeleteValue" @keyup.esc="onEscape" @keydown.up.prevent="typeAheadUp"
          @keydown.down.prevent="typeAheadDown" @keydown.enter.prevent="typeAheadSelect" @blur="onSearchBlur" @focus="onSearchFocus"
          type="search" class="form-control" :disabled="disabled" :placeholder="searchPlaceholder" :tabindex="tabindex" :readonly="!searchable"
          :style="{ width: isValueEmpty ? '100%' : 'auto' }" :id="inputId" aria-label="Search for option"> -->
        <input v-if="filter" ref="search" v-model="search" :placeholder="placeholder">

        <ul :style="{ 'max-height': maxHeight, 'margin': hideResults || !filter ? '0' : '0 0 0.75rem' }">
          <li v-for="(option, index) in filteredOptions" :key="index" :class="{ selected: isOptionSelected(option) }">
            <a @mousedown.prevent="select(option)">
              <slot name="option" v-bind="option">
                {{ getLabel(option) }}
              </slot>
            </a>
          </li>
          <li v-if="!filteredOptions.length" class="no-options">
            <slot name="no-options">{{ noMatching }}</slot>
          </li>
        </ul>
        <slot name="total-results" v-if="filter && !hideResults && filteredOptions.length < mutableOptions.length">
          <span class="total-results">Showing {{ filteredOptions.length }} from {{ mutableOptions.length }} results</span>
        </slot>
      </section>
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
  name: 'i-dropdown',
  components: {},
  data: () => ({
    search: '',
    dropdownOpen: false,
  }),

  /**
   * Clone props into mutable values,
   * attach any event listeners.
   */
  created() {
    this.mutableValue = this.value || this.initial;
    this.mutableOptions = this.options.slice(0);
  },
  watch: {
    /**
     * When the value prop changes, update
     * the internal mutableValue.
     * @param  {mixed} val
     * @return {void}
     */
    value(val) {
      this.mutableValue = val;
    },
  },
  props: {
    /**
     * Contains the currently selected value. Very similar to a
     * `value` attribute on an <input>.
     * @type {Object||String||null}
     */
    value: {
      default: null,
    },

    /**
     * Initial value showing on `i-dropdown`
     * @type {String}
     */
    initial: {
      type: String,
      default: 'i-dropdown',
    },

    /**
     * Equivalent to the `placeholder` attribute on an `<input>`.
     * Used in filter
     * @type {String}
     */
    placeholder: {
      type: String,
      default: 'Type your query',
    },

    /**
     * Alternative message to show when has no matching on filter
     * @type {String}
     */
    noMatching: {
      type: String,
      default: 'Sorry, no matching options.',
    },

    /**
     * An array of strings or objects to be used as dropdown choices.
     * If you are using an array of objects, vue-select will look for
     * a `label` key (ex. [{label: 'This is Foo', value: 'foo'}]). A
     * custom label key can be set with the `label` prop.
     * @type {Array}
     */
    options: {
      type: Array,
      default() {
        return [];
      },
    },

    /**
     * Tells vue-select what key to use when generating option
     * labels when each `option` is an object.
     * @type {String}
     */
    label: {
      type: String,
      default: 'label',
    },

    /**
     * Limit how much results will be avaliable in Dropdown
     * @type {String}
     */
    limit: {
      type: String,
      default: '5',
    },

    /**
     * When True a input search will be avaliable
     * @type {Boolean}
     */
    filter: {
      type: Boolean,
      default: false,
    },

    /**
     * Disable the entire component.
     * @type {Boolean}
     */
    disabled: {
      type: Boolean,
      default: false,
    },

    /**
     * Hide results from Dropdown
     * when filteredOptions is called
     * @type {Boolean}
     */
    hideResults: {
      type: Boolean,
      default: false,
    },

    /**
     * Display element inline
     * its mean, no borders
     * @type {Boolean}
     */
    inline: {
      type: Boolean,
      default: false,
    },

    /**
     * Sets the max-height property on the dropdown list.
     * @deprecated
     * @type {String}
     */
    maxHeight: {
      type: String,
      default: '200px',
    },
  },
  computed: {
    /**
     * Return Selected value to v-model.
     * @return {Object|String}
     */
    model: {
      get() {
        return this.mutableValue;
      },
      set(v) {
        this.$emit('input', v);
      },
    },

    /**
     * The currently displayed options, filtered
     * by the search elements value. If tagging
     * true, the search text will be prepended
     * if it doesn't already exist.
     *
     * @return {array}
     */
    filteredOptions() {
      let options = this.mutableOptions.filter(option => {
        if (typeof option === 'object' && option.hasOwnProperty(this.label)) {
          return (
            option[this.label]
              .toString()
              .toLowerCase()
              .indexOf(this.search.toLowerCase()) > -1
          );
        } else if (
          typeof option === 'object' &&
          !option.hasOwnProperty(this.label)
        ) {
          return console.warn(
            `[vue-select warn]: Label key "option.${this
              .label}" does not exist in options object.\nhttp://sagalbot.github.io/vue-select/#ex-labels`,
          );
        }
        return option.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
      });
      return options.slice(0, +this.limit);
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
        // e.target === this.$refs.selected ||
        e.target === this.$refs.toggle
        // e.target === this.$refs.icon ||
        // e.target === this.$el
      ) {
        this.dropdownOpen = !this.dropdownOpen;
        console.log(this.$refs.search);
        if (this.$refs.search) {
          this.$refs.search.focus();
        }

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

    /**
     * Open Dropdown
     * @param  {Event} e
     * @return {void}
     */
    openDropdown(e) {
      this.search && (this.search = '');
      this.$refs.search && this.$refs.search.focus();

      this.dropdownOpen = true;
    },

    /**
     * Close Dropdown
     * @param  {Event} e
     * @return {void}
     */
    closeDropdown() {
      this.dropdownOpen = false;
    },

    /**
     * Select a given option.
     * @param  {Object|String} option
     * @return {void}
     */
    select(option) {
      this.model = option;
      this.closeDropdown();
    },

    /**
     * Callback to generate the label text. If {option}
     * is an object, returns option[this.label] by default.
     * @type {String}
     * @param  {Object || String} option
     * @return {String}
     */
    getLabel(option) {
      if (typeof option === 'object' && this.label && !!option[this.label])
        return option[this.label];
      return option;
    },

    isOptionSelected(option) {
      return this.mutableValue === option;
    },
  },
};
</script>