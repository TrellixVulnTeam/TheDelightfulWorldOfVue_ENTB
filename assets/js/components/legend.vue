<template>
  <span>
    {{ title }}
  </span>
</template>

<script>
export default {
  name: 'Legend',
  props: {
    title: {
      type: String,
      required: true,
    },
  },
};
</script>
