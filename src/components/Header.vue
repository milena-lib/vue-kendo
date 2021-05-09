<template>
<span>
    {{title}}
    <button
      @click="hidecolumn(field)"
      class="k-button k-primary"
      style="float: right;"
    >
      Hide
    </button>
    </span>
</template>
<script>

export default {
 props: {
        field: String,
        title: String,
        sortable: [Boolean, Object],
    },
    methods: {
        hidecolumn: function(e){
            this.$emit('hidecolumn', e);
        }
    }
}
</script>
