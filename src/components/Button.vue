<template>
  <Button class="Button" 
    @click="$emit('onClick',label)"
    :class="{ double, triple, operation }">
    {{ label }}
  </Button>
</template>

<script>

export default {
  props: {
    label: {},
    operation: { type: Boolean },
    double: { type: Boolean },
    triple: { type: Boolean },
  },
};
</script>

<style>
:root {
  --bg-button: #f0f0f0;
  
}

.Button.double {
  grid-column: span 2;
}
.Button.triple {
  grid-column: span 3;
  
}
.Button.operation {
  background-color: #f0f0f0;
  
}

.panel {
  
}
</style>