<template>
  <div class="dropdown">
    <button
      @click="showDropdown = !showDropdown"
      :class="{ open: showDropdown }">
      {{ data }}
      <i class="fas fa-caret-down"></i>
    </button>
    <div v-show="showDropdown" class="dropdown-content">
      <a
        v-for="option in options"
        :key="option"
        @click="select(option)">{{ option }}
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      required: true
    },

    options: {
      type: Array,
      required: true
    }
  },

  data () {
    return {
      showDropdown: false
    }
  },

  computed: {
    data: {
      get () {
        return this.value
      },
      set (value) {
        this.$emit('input', value)
      }
    }
  },

  methods: {
    select (data) {
      // Sets clicked item to be selected and hides dropdown
      this.data = data
      this.showDropdown = false
    }
  }
}
</script>

<style lang="scss" scoped>
.dropdown {
  float: right;
  margin-right: 180px;
  width: 220px;
  :hover {
    cursor: pointer;
  }
  button {
    background-color: #f7f3eb;
    color: #494949;
    padding: 16px;
    font-size: 15px;
    border-radius: 10px;
    width: 100%;
    text-align: left;
    &.open {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
    }
  }
  .dropdown-content {
    position: absolute;
    background-color: #f7f3eb;
    border-radius: 10px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
    width: 220px;
    border-bottom: 1px solid #d3d3d3;
    border-right: 1px solid #d3d3d3;
    border-left: 1px solid #d3d3d3;
    z-index: 1;
    a {
      color: #494949;
      padding: 12px 16px;
      text-decoration: none;
      display: block;
    }
    a:hover {
      background-color: #742d6c;
      color: #fff;
      transition: ease-in-out .5s;
    }
  }
}
</style>
