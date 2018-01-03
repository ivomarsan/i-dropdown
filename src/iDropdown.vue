<template>
  <div class="i-dropdown" :class="{disabled}">
    <i-tooltip v-if="isTooltip" :is-position="isPosition">{{ isTooltip }}</i-tooltip>
    <div ref="toggle" role="button" @mousedown.prevent="openDropdown" :class="{inline, disabled}" :style="{style}">
      <input value="1" type="hidden" v-model="model">
      <span ref="selected" :style="{ 'color': `${isColor} !important` }">
        {{ getLabel(mutableValue) }}
      </span>

      <i ref="icon" class="icon">
        <slot name="icon">
          <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 768 768">
            <path :style="{ 'fill': isColor }" d="m 438.85712,329.14286 h 329.1429 l -164.5714,164.57143 z"></path>
          </svg>
        </slot>
      </i>
    </div>

    <!-- <slot name="spinner">
      <div class="spinner" v-show="mutableLoading">Loading...</div>
    </slot> -->

    <transition name="fade">
      <section v-show="dropdownOpen" :style="style" ref="dropdown">
        <!-- <input ref="search" v-model="search" @keydown.delete="maybeDeleteValue" @keyup.esc="onEscape" @keydown.up.prevent="typeAheadUp"
          @keydown.down.prevent="typeAheadDown" @keydown.enter.prevent="typeAheadSelect" @blur="onSearchBlur" @focus="onSearchFocus"
          type="search" class="form-control" :disabled="disabled" :placeholder="searchPlaceholder" :tabindex="tabindex" :readonly="!searchable"
          :style="{ width: isValueEmpty ? '100%' : 'auto' }" :id="inputId" aria-label="Search for option"> -->
        <input v-if="filter" ref="search" v-model="search" :placeholder="placeholder">

        <ul :style="{ 'max-height': maxHeight, 'margin': hideResults || !filter ? '0' : '0 0 0.75rem' }">
          <li v-for="(option, index) in filteredOptions" :key="index" :class="{ selected: isOptionSelected(option) }">
            <a @mousedown.prevent="select(option)" :style="{ 'color': isColor }">
              <slot name="option" v-bind="option">
                {{ getLabel(option) }}
              </slot>
            </a>
          </li>
          <li v-if="!filteredOptions.length" class="no-options">
            <slot name="no-options">{{ noMatching }}</slot>
          </li>
        </ul>
        <aside ref="results" v-if="filter && !hideResults && filteredOptions.length < mutableOptions.length">
          <slot name="total-results">
            <span class="total-results">Showing {{ filteredOptions.length }} from {{ mutableOptions.length }} results</span>
          </slot>
        </aside>
      </section>
    </transition>
  </div>
</template>

<style lang="scss" src="./iDropdown.scss" scoped></style>

<script>
import iTooltip from 'i-tooltip';

export default {
  name: 'i-dropdown',
  components: {
    iTooltip,
  },
  data: () => ({
    search: '',
    dropdownOpen: false,
    mutableOptions: [],
  }),

  /**
     * Clone props into mutable values,
     * attach any event listeners.
     */
  created() {
    const initialValueWithReturn =
      this.return && this.options.find(o => o[this.return] === this.value);
    this.mutableValue = initialValueWithReturn || this.initial;
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
      if (this.return) {
        let index = this.mutableOptions.findIndex(i => i[this.return] == val);
        return (this.mutableValue = this.mutableOptions[index]);
      }
      this.mutableValue = val;
    },

    /**
     * Update options list
     * when the father change
     * the value passed
     * into this property
     * @param  {mixed} val
     * @return {void}
     */
    options(val) {
      this.mutableOptions = val;
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
       * Tells what key to use when generating option
       * labels when each `option` is an object.
       * @type {String}
       */
    label: {
      type: String,
      default: 'label',
    },

    /**
     * Return to v-model only a string instead an object.
     * Enter with key name and return this value
     * @type {String}
     */
    return: {
      type: String,
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
       * When True a input search will be avaliable.
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
       * Convert All Exibed Text to UPPERCASE.
       * @type {Boolean}
       */
    uppercase: {
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

    /**
       * Integration with iComponents, personalize colors and tooltips.
       * Also personalization is avaliable with `i-dropdown` class in CSS
       */
    isBackground: String,
    isOutline: String,
    isColor: String,
    isTooltip: String,
    isPosition: String,
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
        if (this.return) return this.$emit('input', v[this.return]);
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
        return (
          option
            .toString()
            .toLowerCase()
            .indexOf(this.search.toLowerCase()) > -1
        );
      });
      return options.slice(0, +this.limit);
    },

    style() {
      return {
        'background-color': this.isBackground,
        'text-transform': this.uppercase ? 'uppercase' : '',
        'border-color': this.isOutline,
        color: this.isColor,
      };
    },
  },
  methods: {
    /**
       * Toggle the visibility of the dropdown menu.
       * Close dropdown when clicked outside.
       * @param  {Event} e
       * @return {void}
       */
    toggleDropdown(e) {
      let isClickOutside =
        e.target != this.$refs.dropdown &&
        e.target != this.$refs.selected &&
        e.target != this.$refs.results &&
        e.target != this.$refs.toggle &&
        e.target != this.$refs.search &&
        e.target != this.$refs.icon &&
        e.target != this.$el;

      isClickOutside && this.closeDropdown();
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
      this.$nextTick(() => {
        this.container = document.querySelectorAll('body')[0];
        if (this.container) {
          // this.$el.addEventListener('mouseleave', this.closeDropdown);
          this.container.addEventListener('click', this.toggleDropdown);
        }
      });
    },

    /**
       * Close Dropdown
       * @param  {Event} e
       * @return {void}
       */
    closeDropdown() {
      this.dropdownOpen = false;
      this.container.removeEventListener('click', this.toggleDropdown, false);
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