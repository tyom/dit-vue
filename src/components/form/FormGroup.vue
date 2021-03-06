<template>
  <div :class="groupClassName">
    <template v-if="variant === 'checkbox'">
      <slot></slot>
      <group-label v-if="label" :optional="optional">{{label}}</group-label>
    </template>
    <template v-else>
      <group-label v-if="label" :optional="optional">{{label}}</group-label>
      <p class="form-hint" v-if="hint">{{hint}}</p>
      <slot></slot>
    </template>
  </div>
</template>

<script>
  import kebabCase from 'lodash/kebabCase'

  export default {
    name: 'form-group',
    props: {
      label: String,
      hint: String,
      inline: Boolean,
      hasError: Boolean,
      variant: String,
      optional: Boolean
    },
    computed: {
      groupId() {
        return kebabCase(this.label) + '-' + this._uid
      },
      groupClassName() {
        return {
          'form-group': true,
          'form-group--inline': this.inline,
          'has-error': this.hasError
        }
      }
    },
    components: {
      // (optional) on label
      GroupLabel: {
        props: ['optional'],
        template: `
          <label class="form-label" :for="$parent.groupId">
            <slot></slot>
            <template v-if="optional">(optional)</template>
          </label>
        `
      }
    }
  }
</script>

<style lang="scss">
  @import "../tools";

  .form-group {
    margin: 30px 0;
    max-width: 50em;
  }

  .form-group--inline {
    .form-label {
      display: inline-block;
    }

    @include media('>phone') {
      .form-control {
        width: auto;
      }
      .form-label {
        margin-right: 10px;
        margin-bottom: 0;
      }

      .form-hint {
        margin-top: 5px;
      }
    }
  }

  .form-group.has-error {
    .form-label {
      color: #b10e1e;
    }

    .form-control {
      border-color: #b10e1e;
    }
  }
</style>
