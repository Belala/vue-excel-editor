<template>
  <div />
</template>

<script>
import moment from 'moment'

export default {
  props: {
    field: {type: String, default: ''},
    label: {type: String, default: null},
    type: {type: String, default: 'string'},
    validate: {type: Function, default: null},
    initStyle: {type: Object, default () {return {}}},
    width: {type: String, default: '100px'},
    invisible: {type: Boolean, default: false},
    readonly: {type: Boolean, default: null},
    textTransform: {type: String, default: null}, // replace uppercase prop
    textAlign: {type: String, default: null},

    keyField: {type: Boolean, default: false},
    sticky: {type: Boolean, default: false},
    change: {type: Function, default: null},

    allowKeys: {type: Array, default () {return null}},
    mandatory: {type: Boolean, default: false},
    lengthLimit: {type: Number, default: 0},
    autocomplete: {type: Boolean, default: null},
    pos: {type: Number, default: 0},
    options: {type: Array, default () {return []}},
    summary: {type: String, default: null},
    toValue: {
      type: Function,
      default (text) {
        switch (this.textTransform) {
          case 'uppercase':
            text = text.toUpperCase()
            break
          case 'lowercase':
            text = text.toLowerCase()
            break
        }
        switch (this.type) {
          case 'datetick':
            return moment(text, 'YYYY-MM-DD').valueOf()
          case 'datetimetick':
            return moment(text, 'YYYY-MM-DD HH:mm').valueOf()
          case 'datetimesectick':
            return moment(text, 'YYYY-MM-DD HH:mm:ss').valueOf()
          case 'check10':
          case 'checkYN':
          case 'checkTF':
            return text.toUpperCase()
          default:
            return text
        }
      }
    },
    toText: {
      type: Function,
      default (val) {
        // § magic to hide the temp key
        if (this.keyField && val && val.startsWith('§')) return ''

        switch (this.type) {
          case 'date':
            return val? moment(val).format('YYYY-MM-DD'): ''
          case 'datetick':
            return moment(Number(val)).format('YYYY-MM-DD')
          case 'datetimetick':
            return moment(Number(val)).format('YYYY-MM-DD HH:mm')
          case 'datetimesectick':
            return moment(Number(val)).format('YYYY-MM-DD HH:mm:ss')
          default:
            return val
        }
      }
    },
    register: {type: Function, default: null}
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      let style = this.initStyle
      let validate = this.validate
      let allowKeys = this.allowKeys
      let lengthLimit = this.lengthLimit

      switch (this.type) {
        case 'number':
          style.textAlign = 'right'
          allowKeys = allowKeys || ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '.', '-']
          break
        case 'date':
          allowKeys = allowKeys || ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '-']
          if (!validate) validate = (val) => {
            if (!moment(val, 'YYYY-MM-DD', true).isValid()) return this.$parent.localizedLabel.invalidInputValue
            return ''
          }
          break
        case 'datetime':
          allowKeys = allowKeys || ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '-', ' ', ':']
          if (!validate) validate = (val) => {
            if (!moment(val, 'YY-MM-DD hh:mm', true).isValid()) return this.$parent.localizedLabel.invalidInputValue
            return ''
          }
          break
        case 'datetimesec':
          allowKeys = allowKeys || ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '-', ' ', ':']
          if (!validate) validate = (val) => {
            if (!moment(val, 'YY-MM-DD hh:mm:ss', true).isValid()) return this.$parent.localizedLabel.invalidInputValue
            return ''
          }
          break
        case 'datetick':
          allowKeys = allowKeys || ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '-', ' ', ':']
          break
        case 'datetimetick':
          allowKeys = allowKeys || ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '-', ' ', ':']
          break
        case 'datetimesectick':
          allowKeys = allowKeys || ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '-', ' ', ':']
          break
        case 'check10':
          style.textAlign = 'center'
          style.textTransform = 'uppercase'
          allowKeys = allowKeys || ['0', '1']
          lengthLimit = lengthLimit || 1
          break
        case 'checkYN':
          style.textAlign = 'center'
          style.textTransform = 'uppercase'
          allowKeys = allowKeys || ['Y', 'N']
          lengthLimit = lengthLimit || 1
          break
        case 'checkTF':
          style.textAlign = 'center'
          style.textTransform = 'uppercase'
          allowKeys = allowKeys || ['T', 'F']
          lengthLimit = lengthLimit || 1
          break
        case 'select':
        case 'string':
          break
        default:
          throw new Error('VueExcelColumn: Not supported type:' + this.type)
      }

      if (this.textTransform) style.textTransform = this.textTransform
      if (this.textAlign) style.textAlign = this.textAlign
      if (this.readonly && this.$parent.readonlyStyle) style = Object.assign(style, this.$parent.readonlyStyle)

      this.$parent.registerColumn({
        name: this.field,
        label: this.label === null ? this.field : this.label,
        type: this.type,
        width: this.width,
        validate: validate,
        change: this.change,

        keyField: this.keyField,
        sticky: this.sticky,
        allowKeys: allowKeys,
        mandatory: this.mandatory,
        lengthLimit: Number(lengthLimit),

        autocomplete: this.autocomplete === null ? this.$parent.autocomplete : this.autocomplete,
        initStyle: style,
        invisible: this.invisible,
        readonly: this.readonly === null ? this.$parent.readonly : this.readonly,
        pos: Number(this.pos),
        options: this.options,
        summary: this.summary,
        toValue: this.toValue,
        toText: this.toText,
        register: this.register
      })
    }
  }
}
</script>
