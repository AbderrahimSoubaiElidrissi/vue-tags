<script>
  export default {
    model: {
      prop: 'tags',
      event: 'update'
    },
    props: {
      deleteKey: {
      type: Array,
      default: function() {
          return ['8']
        }
      },
      addKey: {
      type: Array,
      default: function() {
          return ['13']
        }
      },
      tags:{
        required: true
      }
    },
    data() {
      return {
        newTag: '',
      }
    },
    methods: {
      handleTagBackspace(e) {
        if (this.newTag.length === 0) {
          this.$emit('update', this.tags.slice(0, -1))
        }
      },
      addTag() {
        if (this.newTag.length === 0 || this.tags.includes(this.newTag)) {
          return
        }

        this.$emit('update', [...this.tags, this.newTag])
        this.newTag = ''
      },
      removeTag(tag) {
        this.$emit('update', this.tags.filter(t => t !== tag))
      }
    },
    render() {
      return this.$scopedSlots.default({
        tags : this.tags,
        removeTag: this.removeTag,
        inputEventHandlers: {
          input : (e) => {this.newTag = e.target.value},
          keydown : (e) => {

          if(this.deleteKey.includes(e.keyCode.toString()))
          {
            this.handleTagBackspace()
          }

          if(this.addKey.includes(e.keyCode.toString()))
          {
            e.preventDefault()
            this.addTag()
          }

          if(e.keyCode === 13 && this.deleteKey !== 13)
          {
            e.preventDefault()
          }
        }
        },
        inputBindings: {
          value : this.newTag
        },
      })
    }
  }
</script>
